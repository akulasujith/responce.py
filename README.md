import datetime
import logging
import jwt
from flask import request, jsonify
from functools import wraps
import globalvars as gvar
import Services.dboperations as dbops
import unittest
from unittest.mock import patch, MagicMock

class TestAuth(unittest.TestCase):

    def setUp(self):
        # Initialize globalvars for each test
        gvar.func = None
        gvar.user_id = None
        gvar.user_name = None
        gvar.user_id_short = None
        gvar.user_ip_address = None
        gvar.ISAUTHORIZED = False
        gvar.sadrdUsersList = []

    def tearDown(self):
        # Cleanup after each test
        pass

    def test_DecryptToken_valid(self):
        token = "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1bmlxdWVfbmFtZSI6InRlc3QifQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
        decoded_token = jwt.decode(token[7:].encode('utf-8'), options={"verify_signature": False})
        result = DecryptToken(token)
        self.assertEqual(result, decoded_token)

    def test_GetLoggedInUser_with_domain(self):
        token = "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1bmlxdWVfbmFtZSI6InRlc3RAU01GQ0dELkNPTSJ9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
        GetLoggedInUser(token)
        self.assertEqual(gvar.user_id, "test")

    def test_GetLoggedInUser_without_domain(self):
        token = "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9eyJ1bmlxdWVfbmFtZSI6InRlc3QifQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
        GetLoggedInUser(token)
        self.assertEqual(gvar.user_id, "test")

    @patch('flask.request')
    @patch('Auth.DecryptToken')
    @patch('Auth.GetLoggedInUser')
    def test_token_required_valid_token_authorized(self, mock_get_logged_in_user, mock_decrypt_token, mock_request):
        mock_request.headers = {'Authorization': 'Bearer valid_token'}
        mock_decrypt_token.return_value = {'name': 'Test User', 'ipaddr': '127.0.0.1'}
        mock_get_logged_in_user.return_value = 'testuser'
        gvar.sadrdUsersList = [MagicMock(NetworkId='testuser')]

        @token_required
        def dummy_function():
            return "OK"

        result = dummy_function()
        self.assertEqual(result, "OK")
        self.assertTrue(gvar.ISAUTHORIZED)

    @patch('flask.request')
    @patch('Auth.DecryptToken')
    @patch('Auth.GetLoggedInUser')
    def test_token_required_valid_token_unauthorized(self, mock_get_logged_in_user, mock_decrypt_token, mock_request):
        mock_request.headers = {'Authorization': 'Bearer valid_token'}
        mock_decrypt_token.return_value = {'name': 'Test User', 'ipaddr': '127.0.0.1'}
        mock_get_logged_in_user.return_value = 'testuser'
        gvar.sadrdUsersList = [MagicMock(NetworkId='otheruser')]

        @token_required
        def dummy_function():
            return "OK"

        result = dummy_function()
        self.assertEqual(result, jsonify({'apirespmsg': 'Unauthorized Access'}))
        self.assertFalse(gvar.ISAUTHORIZED)

    @patch('flask.request')
    @patch('Auth.DecryptToken')
    @patch('Auth.GetLoggedInUser')
    def test_token_required_valid_token_empty_user_list(self, mock_get_logged_in_user, mock_decrypt_token, mock_request):
        mock_request.headers = {'Authorization': 'Bearer valid_token'}
        mock_decrypt_token.return_value = {'name': 'Test User', 'ipaddr': '127.0.0.1'}
        mock_get_logged_in_user.return_value = 'testuser'
        gvar.sadrdUsersList = []

        @token_required
        def dummy_function():
            return "OK"

        result = dummy_function()
        self.assertEqual(result, jsonify({'apirespmsg': 'Unauthorized Access'}))
        self.assertFalse(gvar.ISAUTHORIZED)

    @patch('flask.request')
    @patch('Auth.DecryptToken')
    @patch('Auth.GetLoggedInUser')
    @patch('Services.dboperations.dboperations.insert_actionLog')
    def test_token_required_invalid_token(self, mock_insert_action_log, mock_get_logged_in_user, mock_decrypt_token, mock_request):
        mock_request.headers = {'Authorization': 'Bearer invalid_token'}
        mock_decrypt_token.side_effect = Exception("Invalid token")
        mock_get_logged_in_user.return_value = 'testuser'

        @token_required
        def dummy_function():
            return "OK"

        result = dummy_function()
        self.assertEqual(result, (jsonify({'apirespmsg': 'Invalid Token!'}), 494))
        mock_insert_action_log.assert_called_once()

    @patch('flask.request')
    def test_token_required_missing_token(self, mock_request):
        mock_request.headers = {}

        @token_required
        def dummy_function():
            return "OK"

        result = dummy_function()
        self.assertEqual(result, jsonify({'apirespmsg': 'Authorization Token is missing!'}))
        self.assertFalse(gvar.ISAUTHORIZED)

if __name__ == '__main__':
    logging.basicConfig(level=logging.DEBUG)
    unittest.main(argv=['first-arg-is-ignored'], exit=False)
