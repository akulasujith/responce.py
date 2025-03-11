import pytest
from unittest.mock import patch, MagicMock
from flask import Flask, request, jsonify
from APIHome import mainapp, dbops_obj, auth, AdminAuthorize
from Entities.Customentities import ApihomeResp
import datetime

@pytest.fixture
def client():
    mainapp.config['TESTING'] = True
    mainapp.config['WTF_CSRF_ENABLED'] = False
    with mainapp.test_client() as client:
        yield client

@pytest.fixture
def mock_dbops():
    with patch('Services.dboperations.dboperations') as mock:
        yield mock

@pytest.fixture
def mock_auth():
    with patch('Services.Auth.Auth') as mock:
        mock.token_required.return_value = lambda f: f
        yield mock

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_authenticate_user_success(client, mock_dbops):
    mock_user = MagicMock()
    mock_user.NetworkId = 'testuser'
    mock_user.Name = 'Test User'
    mock_user.RoleId = 1
    mock_user.isActive = True
    mock_user.Email = 'test@example.com'
    mock_dbops.GetAllUsers.return_value = [mock_user]
    mock_dbops.GetAllRoles.return_value = [{'RoleID': 1, 'Type': 'Admin'}]
    with patch('APIHome.auth.GetLoggedInUser') as mock_get_user:
        mock_get_user.return_value = 'testuser'
        headers = {'Authorization': 'Bearer validtoken'}
        response = client.get('/api/AuthenticateUser', headers=headers)
    assert response.status_code == 200
    assert response.json['authenticated'] is True
    assert response.json['Name'] == 'Test User'

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_authenticate_user_failure_no_auth_header(client, mock_dbops):
    with patch('APIHome.auth.GetLoggedInUser') as mock_get_user:
        mock_get_user.return_value = 'testuser'
        response = client.get('/api/AuthenticateUser')
    assert response.status_code == 200
    assert response.json['authenticated'] is False

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_authenticate_user_failure_no_users(client, mock_dbops):
    mock_dbops.GetAllUsers.return_value =
    with patch('APIHome.auth.GetLoggedInUser') as mock_get_user:
        mock_get_user.return_value = 'testuser'
        headers = {'Authorization': 'Bearer validtoken'}
        response = client.get('/api/AuthenticateUser', headers=headers)
    assert response.status_code == 200
    assert response.json['authenticated'] is False

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_get_import_types(client, mock_dbops):
    mock_setting = MagicMock()
    mock_setting.settingName = 'ImportType'
    mock_setting.settingValue = 'Type1'
    mock_setting.Description = 'Test Import Type'
    mock_dbops.SadrdSysSettings.return_value = [mock_setting]
    response = client.get('/api/GetImportTypes')
    assert response.status_code == 200
    assert len(response.json['ImportTypesData']) == 1
    assert response.json['ImportTypesData'][0]['ImportType'] == 'Type1'

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
@patch('Services.parentparser.parentparser')
def test_import_data_success(mock_parser, client, mock_dbops):
    mock_parser.return_value = ApihomeResp(status="Success", message="Imported successfully")
    mock_dbops.SadrdSysSettings.return_value = [MagicMock(settingName="ServerFolderPath", settingValue="/test/path")]
    data = {'year': '2023', 'importType': 'TestType'}
    response = client.post('/api/ImportData', data=data)
    assert response.status_code == 200
    assert response.json['status'] == "Success"

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_get_action_logs(client, mock_dbops):
    mock_log = MagicMock()
    mock_log.LogID = 1
    mock_log.Action = 'Test Action'
    mock_dbops.get_actionLog.return_value = [mock_log]
    response = client.get('/api/GetActionLogs')
    assert response.status_code == 200
    assert len(response.json['ActionLogsData']) > 0

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
@patch('Services.dboperations.dboperations.UpdateUser')
def test_update_user_success(mock_update, client):
    mock_update.return_value = "Success"
    data = {
        'NetworkId': 'testuser',
        'Name': 'New User',
        'RoleId': 2,
        'isActive': True,
        'Email': 'new@example.com',
        'userAction': 'add'
    }
    response = client.post('/api/UpdateUser', data=data)
    assert response.status_code == 200
    assert response.json['status'] == "Success"

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_cusip_mapping_data_not_generated(client, mock_dbops):
    mock_dbops.SadrdSysSettings.return_value = [
        MagicMock(settingName="SADRD_ReportGenerated", settingValue="N")
    ]
    response = client.get('/api/CusipMappingData')
    assert response.status_code == 200
    assert response.json['status'] == "Failure"

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_admin_authorize_true(client, mock_dbops):
    mock_user = MagicMock()
    mock_user.RoleId = 1
    mock_user.NetworkId = 'admin'
    mock_request = {'NetworkId': 'admin', 'Role': 'Admin', 'RoleId': '1'}
    mock_dbops.GetAllUsers.return_value = [mock_user]
    with patch('APIHome.GetAllRoles') as mock_get_roles:
        mock_get_roles.return_value = jsonify({'RolesData': [{'RoleID': 1, 'Type': 'Admin'}]})
        response = client.post('/api/ReturnNetworkFilePath', data=mock_request)
        assert response.status_code == 200

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_admin_authorize_false(client, mock_dbops):
    mock_user = MagicMock()
    mock_user.RoleId = 2  # Not an admin role
    mock_user.NetworkId = 'user'
    mock_request = {'NetworkId': 'user', 'Role': 'User', 'RoleId': '2'}
    mock_dbops.GetAllUsers.return_value = [mock_user]
    with patch('APIHome.GetAllRoles') as mock_get_roles:
        mock_get_roles.return_value = jsonify({'RolesData': [{'RoleID': 2, 'Type': 'User'}]})
        response = client.post('/api/ReturnNetworkFilePath', data=mock_request)
        assert response.status_code != 200

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_get_all_user_details(client, mock_dbops):
    mock_user = MagicMock()
    mock_user.NetworkId = 'testuser'
    mock_user.Name = 'Test User'
    mock_user.isActive = True
    mock_user.RoleId = 1
    mock_user.Email = 'test@example.com'
    mock_dbops.GetAllUsers.return_value = [mock_user]
    with patch('APIHome.GetAllRoles') as mock_get_roles:
        mock_get_roles.return_value = jsonify({'RolesData': [{'RoleID': 1, 'Type': 'Admin'}]})
        response = client.get('/api/GetAllUserDetails')
        assert response.status_code == 200
        assert len(response.json['UsersData']) == 1

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_get_all_roles(client, mock_dbops):
    mock_role = MagicMock()
    mock_role.RoleID = 1
    mock_role.Type = 'Admin'
    mock_dbops.GetAllRoles.return_value = [mock_role]
    response = client.get('/api/GetAllRoles')
    assert response.status_code == 200
    assert len(response.json['RolesData']) == 1

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_return_tax_year(client, mock_dbops):
    mock_dbops.SadrdSysSettings.return_value = [MagicMock(settingName="SADRD_Year", settingValue="2023")]
    response = client.get('/api/GetTaxYear')
    assert response.status_code == 200
    assert response.data.decode('utf-8') == "2023"

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_return_last_process_sadrd_run(client, mock_dbops):
    mock_dbops.SadrdSysSettings.return_value = [MagicMock(settingName="LastProcessSADRDRun", settingValue="2023-03-13 10:00:00.000")]
    response = client.get('/api/GetLastProcessSADRDRun')
    assert response.status_code == 200
    assert response.data.decode('utf-8') == "2023-03-13 10:00:00.000"

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_close_tax_year_success(client, mock_dbops):
    mock_dbops.SadrdSysSettings.return_value = [
        MagicMock(settingName="SADRD_ReportGenerated", settingValue="Y"),
        MagicMock(settingName="SADRD_Year", settingValue="2023"),
        MagicMock(settingName="ServerFolderPath", settingValue="test_folder")
    ]
    mock_dbops.CloseTaxYear.return_value = "Tax year closed successfully"
    with patch('APIHome.shutil.move') as mock_move:
        response = client.post('/api/CloseTaxYear', data='2024')
        assert response.status_code == 200
        assert response.json['status'] == "Success"
        mock_move.assert_called()

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_close_tax_year_failure(client, mock_dbops):
    mock_dbops.SadrdSysSettings.return_value = [
        MagicMock(settingName="SADRD_ReportGenerated", settingValue="N")
    ]
    response = client.post('/api/CloseTaxYear', data='2024')
    assert response.status_code == 200
    assert response.json['status'] == "Failure"

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_get_network_file_path_admin(client, mock_dbops):
    mock_dbops.SadrdSysSettings.return_value = [MagicMock(settingName="ServerFolderPath", settingValue="/test/path")]
    mock_request = {'NetworkId': 'admin', 'Role': 'Admin', 'RoleId': '1'}
    with patch('APIHome.AdminAuthorize') as mock_admin_authorize:
        mock_admin_authorize.return_value = True
        response = client.post('/api/GetNetworkFilePath', data=mock_request)
        assert response.status_code == 200
        assert response.json['status'] == "Success"
        assert response.json['message'] == "/test/path"

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_get_network_file_path_unauthorized(client, mock_dbops):
    mock_request = {'NetworkId': 'user', 'Role': 'User', 'RoleId': '2'}
    with patch('APIHome.AdminAuthorize') as mock_admin_authorize:
        mock_admin_authorize.return_value = False
        response = client.post('/api/GetNetworkFilePath', data=mock_request)
        assert response.status_code == 200
        assert response.json['status'] == "Unauthorized"

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_set_default_file_location_admin(client, mock_dbops):
    mock_dbops.UpdateFilePath.return_value = "Success"
    mock_request = {'NetworkId': 'admin', 'Role': 'Admin', 'RoleId': '1', 'newFilePath': '/new/path'}
    with patch('APIHome.AdminAuthorize') as mock_admin_authorize:
        mock_admin_authorize.return_value = True
        response = client.post('/api/SetDefaultFileLocation', data=mock_request)
        assert response.status_code == 200
        assert response.json['status'] == "Success"

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_set_default_file_location_unauthorized(client, mock_dbops):
    mock_request = {'NetworkId': 'user', 'Role': 'User', 'RoleId': '2', 'newFilePath': '/new/path'}
    with patch('APIHome.AdminAuthorize') as mock_admin_authorize:
        mock_admin_authorize.return_value = False
        response = client.post('/api/SetDefaultFileLocation', data=mock_request)
        assert response.status_code == 200
        assert response.json['status'] == "Unauthorized"

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_get_settings_data(client, mock_dbops):
    mock_setting = MagicMock()
    mock_setting.settingName = 'TestSetting'
    mock_setting.settingValue = 'TestValue'
    mock_setting.ShowInUI = True
    mock_setting.Description = 'Test Description'
    mock_setting.DataType = 'string'
    mock_dbops.SadrdSysSettings.return_value = [mock_setting]
    response = client.get('/api/GetSettingsData')
    assert response.status_code == 200
    assert len(response.json['SettingsData']) == 1

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_update_settings_data_success(client, mock_dbops):
    mock_dbops.UpdateSettingsData.return_value = "Success"
    data = {
        'settingName': 'TestSetting',
        'settingValue': 'NewValue',
        'userAction': 'update'
    }
    response = client.post('/api/UpdateSettingsData', data=data)
    assert response.status_code == 200
    assert response.json['status'] == "Success"

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_get_adm_schedule_e_data(client, mock_dbops):
    mock_schedule = MagicMock()
    mock_schedule.Cusip = '123456789'
    mock_schedule.CusipName = 'Test Cusip'
    mock_dbops.SadrdAdmScheduleE.return_value = [mock_schedule]
    response = client.get('/api/GetAdmScheduleEData')
    assert response.status_code == 200
    assert len(response.json['ScheduleEData']) == 1

@patch.dict('APIHome.mainapp.config', {'ENV': 'production'})
def test_update_adm_schedule_e_data_success(client, mock_dbops):
    mock_dbops.UpdateAdmScheduleEData.return_value = "Success"
    data = {
        'Cusip': '123456789',
        'CusipName': 'Updated Cusip Name',
        'userAction': 'update'
    }
    response = client.post('/api/UpdateAdmScheduleEData', data=data
