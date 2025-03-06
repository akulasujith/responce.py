import unittest
from unittest.mock import patch, MagicMock
import datetime
import jwt
from flask import Flask, request, jsonify
from functools import wraps
import globalvars as gvar
import Services.dboperations as dbops
import os

# Assuming globalvars.py and Services/dboperations.py are in the same directory or accessible via PYTHONPATH
# Mocking globalvars and dboperations for testing
gvar.sadrdUsersList = []
gvar.user_id = ''
gvar.user_name = ''
gvar.user_id_short = ''
gvar.user_ip_address = ''
gvar.ISAUTHORIZED = False
gvar.func = ''

class MockDBOperations:
    def insert_actionLog(self, month, year, user_id, func_name, action_desc, action_time, error_desc, other_details):
        pass

def DecryptToken(token):
    data = str(token)[7:]
    data = data.encode('utf-8')
    decoded_token = jwt.decode(data, options={"verify_signature": False})
    return decoded_token

def GetLoggedInUser(token):
    decrypted_token = DecryptToken(token)
    loggedInUser = (str(decrypted_token['unique_name']))
    if loggedInUser.find('@MFCGD.COM') > 0:
        gvar.user_id = loggedInUser[0:loggedInUser.find('@MFCGD.COM')]
    else:
        gvar.user_id = loggedInUser
    return gvar.user_id

def token_required(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        gvar.func = f.__name__
        gvar.user_id = ''
        gvar.user_name = ''
        gvar.user_id_short = ''
        gvar.user_ip_address = ''

        if (request.headers.__contains__('Authorization')):
            data = request.headers['Authorization']
            try:
                decrypted_token = DecryptToken(data)    
                gvar.user_id_short = GetLoggedInUser(data)
                gvar.user_name = str(decrypted_token['name'])
                gvar.user_ip_address = str(decrypted_token['ipaddr'])
                
                if len(gvar.sadrdUsersList):
                    datatb = [x for x in gvar.sadrdUsersList if x.NetworkId == gvar.user_id_short]
                    if len(datatb):
                        gvar.ISAUTHORIZED = True
                    else:
                        gvar.ISAUTHORIZED = False
                        return jsonify({'apirespmsg': 'Unauthorized Access'})
                else:
                    gvar.ISAUTHORIZED = False
                    return jsonify({'apirespmsg': 'Unauthorized Access'})
            except Exception as e:
                dbops_obj = MockDBOperations()
                dbops_obj.insert_actionLog(datetime.datetime.now().month, datetime.datetime.now().year, gvar.user_id, 'token_required', 'token_required', str(datetime.datetime.now())[0:23], ("Exception occured in token_required() :: " + str(e)), None)
                return jsonify({'apirespmsg' : 'Invalid Token!'}), 494
            return f(*args, **kwargs)
        else:
            gvar.ISAUTHORIZED = False
            return jsonify({'apirespmsg': 'Authorization Token is missing!'})

    return decorated

class TestAuthFunctions(unittest.TestCase):

    def setUp(self):
        self.app = Flask(__name__)
        self.test_token = "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1bmlxdWVfbmFtZSI6InRlc3R1c2VyQG1mY2dkLmNvbSIsIm5hbWUiOiJUZXN0IFVzZXIiLCJpcGFkZHIiOiIxMjcuMC4wLjEifQ.dGVzdHNpZ25hdHVyZQ"
        self.test_token_no_domain = "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1bmlxdWVfbmFtZSI6InRlc3R1c2VyIiwibmFtZSI6IlRlc3QgVXNlciIsImlwYWRkciI6IjEyNy4wLjAuMSJ9.dGVzdHNpZ25hdHVyZQ"
        self.unauthorized_token = "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1bmlxdWVfbmFtZSI6InVudXNlckBtZmNnZC5jb20iLCJuYW1lIjoiVW5hdXRob3JpemVkIFVzZXIiLCJpcGFkZHIiOiIxMjcuMC4wLjEifQ.dGVzdHNpZ25hdHVyZQ"
        gvar.sadrdUsersList = []
        gvar.user_id = ''
        gvar.user_name = ''
        gvar.user_id_short = ''
        gvar.user_ip_address = ''
        gvar.ISAUTHORIZED = False
        gvar.func = ''

    def test_DecryptToken(self):
        decoded_token = DecryptToken(self.test_token)
        self.assertEqual(decoded_token['unique_name'], 'testuser@mfcgd.com')

    def test_GetLoggedInUser_with_domain(self):
        user_id = GetLoggedInUser(self.test_token)
        self.assertEqual(user_id, 'testuser')

    def test_GetLoggedInUser_without_domain(self):
        user_id = GetLoggedInUser(self.test_token_no_domain)
        self.assertEqual(user_id, 'testuser')

    def test_token_required_success(self):
        @self.app.route('/test')
        @token_required
        def test_route():
            return jsonify({'message': 'success'})

        gvar.sadrdUsersList = [MagicMock(NetworkId='testuser')]
        with self.app.test_client() as client:
            response = client.get('/test', headers={'Authorization': self.test_token})
            self.assertEqual(response.status_code, 200)
            self.assertEqual(response.json['message'], 'success')
            self.assertTrue(gvar.ISAUTHORIZED)

    def test_token_required_unauthorized(self):
        @self.app.route('/test')
        @token_required
        def test_route():
            return jsonify({'message': 'success'})

        gvar.sadrdUsersList = [MagicMock(NetworkId='testuser')]
        with self.app.test_client() as client:
            response = client.get('/test', headers={'Authorization': self.unauthorized_token})
            self.assertEqual(response.status_code, 200)
            self.assertEqual(response.json['apirespmsg'], 'Unauthorized Access')
            self.assertFalse(gvar.ISAUTHORIZED)

    def test_token_required_no_token(self):
        @self.app.route('/test')
        @token_required
        def test_route():
            return jsonify({'message': 'success'})

        with self.app.test_client() as client:
            response = client.get('/test')
            self.assertEqual(response.status_code, 200)
            self.assertEqual(response.json['apirespmsg'], 'Authorization Token is missing!')
            self.assertFalse(gvar.ISAUTHORIZED)

    def test_token_required_invalid_token(self):
        @self.app.route('/test')
        @token_required
        def test_route():
            return jsonify({'message': 'success'})

        with self.app.test_client() as client:
            response = client.get('/test', headers={'Authorization': 'Bearer invalid_token'})
            self.assertEqual(response.status_code, 494)
            self.assertEqual(response.json['apirespmsg'], 'Invalid Token!')
            self.assertFalse(gvar.ISAUTHORIZED)

    def test_token_required_empty_user_list(self):
        @self.app.route('/test')
        @token_required
        def test_route():
            return jsonify({'message': 'success'})

        with self.app.test_client() as client:
            response = client.get('/test', headers={'Authorization': self.test_token})
            self.assertEqual(response.status_code,
