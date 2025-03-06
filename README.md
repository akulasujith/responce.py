import unittest
from unittest.mock import MagicMock
from flask import Flask, jsonify

import Services.APIResponse as apir

class TestApirespHandler(unittest.TestCase):
    def setUp(self):
        self.app = Flask(__name__)
        self.app_context = self.app.app_context()
        self.app_context.push()
        self.handler = apir.ApirespHandler()

    def tearDown(self):
        self.app_context.pop()

    def test_getResponse_empty_resp(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {}
        self.handler.setResponse(mock_resp)
        response = self.handler.getResponse()
        self.assertEqual(response.status_code, 200)

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
        self.handler.setResponse(None)
        response = self.handler.getResponse()
        self.assertEqual(response.status_code, 200)
        self.assertEqual(response.get_json(), {})

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
        self.assertNotIn("channelname", response.get_json())
        self.assertNotIn("channelurl", response.get_json())

    def test_getResponse_without_channel_info(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {"key1": "value1", "key2": 123}
        self.handler.setResponse(mock_resp)
        response = self.handler.getResponse()
        self.assertEqual(response.status_code, 200)

    def test_setResponse_invalid_input(self):
        with self.assertRaises(AttributeError):
            self.handler.setResponse("invalid_input")

class TestApirespBase(unittest.TestCase):
    def setUp(self):
        self.app = Flask(__name__)
        self.app_context = self.app.app_context()
        self.app_context.push()

    def tearDown(self):
        self.app_context.pop()

    def test_getResponse_abstract(self):
        with self.assertRaises(NotImplementedError):
            apir.ApirespBase().getResponse()

    def test_setResponse_abstract(self):
        with self.assertRaises(NotImplementedError):
            apir.ApirespBase().setResponse(None)
