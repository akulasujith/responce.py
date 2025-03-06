import unittest
from unittest.mock import MagicMock
from flask import Flask, jsonify

# Assuming your APIResponse.py has classes ApirespHandler and ApirespBase
from Services import APIResponse as apir

class TestApirespHandler(unittest.TestCase):
    def setUp(self):
        self.app = Flask(__name__)  # Create a Flask app instance
        self.app_context = self.app.app_context() #create app context
        self.app_context.push() # push app context
        self.handler = apir.ApirespHandler()

    def tearDown(self):
        self.app_context.pop() # pop app context

    def test_getResponse_empty_resp(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {}
        self.handler.setResponse(mock_resp)
        response = self.handler.getResponse()
        self.assertEqual(response.status_code, 200) #add assertion for status code.

    def test_getResponse_invalid_json(self):
        class NonSerializable:
            pass

        mock_resp = MagicMock()
        mock_resp.__dict__ = {"key": NonSerializable()}
        self.handler.setResponse(mock_resp)
        with self.assertRaises(TypeError):
            self.handler.getResponse()

    def test_getResponse_nested_data(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {
            "key1": {"nested_key": "value"},
            "key2": [1, 2, {"nested_key": "value"}]
        }
        self.handler.setResponse(mock_resp)
        response = self.handler.getResponse()
        self.assertEqual(response.status_code, 200)

    def test_getResponse_no_dict(self):
        mock_resp = MagicMock()
        del mock_resp.__dict__
        self.handler.setResponse(mock_resp)
        with self.assertRaises(AttributeError):
            self.handler.getResponse()

    def test_getResponse_non_string_keys(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {123: "value"}
        self.handler.setResponse(mock_resp)
        response = self.handler.getResponse()
        self.assertEqual(response.status_code, 200)

    def test_getResponse_none_values(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {"key1": None, "key2": "value"}
        self.handler.setResponse(mock_resp)
        response = self.handler.getResponse()
        self.assertEqual(response.status_code, 200)

    def test_getResponse_with_channel_info(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {
            "key1": "value1",
            "key2": 123,
            "channelname": "test",
            "channelurl": "http://test",
            "list_data": [1, 2, 3],
            "bool_data": True
        }
        self.handler.setResponse(mock_resp)
        response = self.handler.getResponse()
        self.assertEqual(response.status_code, 200)

    def test_getResponse_without_channel_info(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {"key1": "value1", "key2": 123}
        self.handler.setResponse(mock_resp)
        response = self.handler.getResponse()
        self.assertEqual(response.status_code, 200)

    def test_setResponse_invalid_input(self):
        with self.assertRaises(TypeError):
            self.handler.setResponse("invalid_input")

class TestApirespBase(unittest.TestCase):
    def setUp(self):
        self.base = apir.ApirespBase() #instantiate ApirespBase
        self.app = Flask(__name__)  # Create a Flask app instance
        self.app_context = self.app.app_context() #create app context
        self.app_context.push() # push app context

    def tearDown(self):
        self.app_context.pop() # pop app context

    def test_getResponse(self):
        with self.assertRaises(NotImplementedError):
            self.base.getResponse()

    def test_setResponse(self):
        with self.assertRaises(NotImplementedError):
            self.base.setResponse(None) #add a placeholder so the function can be called.
