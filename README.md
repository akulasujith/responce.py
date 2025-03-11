import pytest
from unittest.mock import patch, MagicMock
from flask import Flask, url_for, json
import db
import globalvars as gvar
from apihome import mainapp, dbops_obj, auth, AdminAuthorize # type: ignore
from Entities.Customentities import ApihomeResp

@pytest.fixture
def client():
    mainapp.config['TESTING'] = True
    mainapp.config['WTF_CSRF_ENABLED'] = False
    with mainapp.test_client() as client:
        with mainapp.app_context():
            db.init_app(mainapp)
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

def test_authenticate_user_success(client, mock_dbops):
    # Mock valid user data
    mock_user = MagicMock()
    mock_user.NetworkId = 'testuser'
    mock_user.Name = 'Test User'
    mock_user.RoleId = 1
    mock_user.isActive = True
    mock_user.Email = 'test@example.com'
    mock_dbops.GetAllUsers.return_value = [mock_user]
    mock_dbops.GetAllRoles.return_value = [{'RoleID': 1, 'Type': 'Admin'}]

    # Mock request headers with valid token
    headers = {'Authorization': 'Bearer validtoken'}
    response = client.get('/api/AuthenticateUser', headers=headers)
    
    assert response.status_code == 200
    assert response.json['authenticated'] is True
    assert response.json['Name'] == 'Test User'

def test_authenticate_user_failure(client, mock_dbops):
    # Mock no users found
    mock_dbops.GetAllUsers.return_value = []
    headers = {'Authorization': 'Bearer invalidtoken'}
    response = client.get('/api/AuthenticateUser', headers=headers)
    
    assert response.status_code == 200
    assert response.json['authenticated'] is False

def test_get_import_types(client, mock_dbops):
    # Mock settings data
    mock_setting = MagicMock()
    mock_setting.settingName = 'ImportType'
    mock_setting.settingValue = 'Type1'
    mock_setting.Description = 'Test Import Type'
    mock_dbops.SadrdSysSettings.return_value = [mock_setting]
    
    response = client.get('/api/GetImportTypes')
    
    assert response.status_code == 200
    assert len(response.json['ImportTypesData']) == 1
    assert response.json['ImportTypesData'][0]['ImportType'] == 'Type1'

@patch('Services.parentparser.parentparser')
def test_import_data_success(mock_parser, client, mock_dbops):
    # Mock successful import
    mock_parser.return_value = ApihomeResp(status="Success", message="Imported successfully")
    mock_dbops.SadrdSysSettings.return_value = [MagicMock(settingName="ServerFolderPath", settingValue="/test/path")]
    
    data = {'year': '2023', 'importType': 'TestType'}
    response = client.post('/api/ImportData', data=data)
    
    assert response.status_code == 200
    assert response.json['status'] == "Success"

def test_get_action_logs(client, mock_dbops):
    # Mock action logs
    mock_log = MagicMock()
    mock_log.LogID = 1
    mock_log.Action = 'Test Action'
    mock_dbops.get_actionLog.return_value = [mock_log]
    
    response = client.get('/api/GetActionLogs')
    
    assert response.status_code == 200
    assert len(response.json['ActionLogsData']) > 0

@patch('Services.dboperations.dboperations.UpdateUser')
def test_update_user_success(mock_update, client):
    # Mock user update
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

def test_cusip_mapping_data_not_generated(client, mock_dbops):
    # Mock report not generated
    mock_dbops.SadrdSysSettings.return_value = [
        MagicMock(settingName="SADRD_ReportGenerated", settingValue="N")
    ]
    response = client.get('/api/CusipMappingData')
    
    assert response.status_code == 200
    assert response.json['status'] == "Failure"

def test_admin_authorize_true():
    # Mock admin user
    mock_user = MagicMock()
    mock_user.RoleId = 1
    mock_user.NetworkId = 'admin'
    mock_request = {'NetworkId': 'admin', 'Role': 'Admin', 'RoleId': '1'}
    
    with patch('apihome.dbops_obj.GetAllUsers') as mock_users:
        mock_users.return_value = [mock_user]
        assert AdminAuthorize(mock_request) is True

# Add similar tests for other routes following the same pattern
