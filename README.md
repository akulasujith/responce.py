import unittest
from unittest.mock import MagicMock, patch
import json
import Services.APIResponse as apir  # Replace with your actual import

class TestApirespHandler(unittest.TestCase):
    def setUp(self):
        self.handler = apir.ApirespHandler()

    def test_setResponse(self):
        mock_resp = MagicMock()
        self.handler.setResponse(mock_resp)
        self.assertEqual(self.handler._respjson, mock_resp)

    def test_setResponse_invalid_input(self):
        with self.assertRaises(TypeError):
            self.handler.setResponse("invalid_input")

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
        response_data = json.loads(response.get_data(as_text=True))
        self.assertNotIn('channelname', response_data)
        self.assertNotIn('channelurl', response_data)
        self.assertEqual(response_data['key1'], 'value1')
        self.assertEqual(response_data['key2'], 123)
        self.assertEqual(response_data['list_data'], [1, 2, 3])
        self.assertEqual(response_data['bool_data'], True)

    def test_getResponse_without_channel_info(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {"key1": "value1", "key2": 123}
        self.handler.setResponse(mock_resp)
        response = self.handler.getResponse()
        response_data = json.loads(response.get_data(as_text=True))
        self.assertEqual(response_data['key1'], 'value1')
        self.assertEqual(response_data['key2'], 123)

    def test_getResponse_empty_resp(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {}
        self.handler.setResponse(mock_resp)
        response = self.handler.getResponse()
        response_data = json.loads(response.get_data(as_text=True))
        self.assertEqual(response_data, {})

    def test_getResponse_none_resp(self):
        self.handler.setResponse(None)
        with self.assertRaises(AttributeError):
            self.handler.getResponse()

    def test_getResponse_no_dict(self):
        mock_resp = MagicMock()
        del mock_resp.__dict__  # Simulate a response without __dict__
        self.handler.setResponse(mock_resp)
        with self.assertRaises(AttributeError):
            self.handler.getResponse()

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
        response_data = json.loads(response.get_data(as_text=True))
        self.assertEqual(response_data["key1"]["nested_key"], "value")
        self.assertEqual(response_data["key2"][2]["nested_key"], "value")

    def test_getResponse_non_string_keys(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {123: "value"}
        self.handler.setResponse(mock_resp)
        response = self.handler.getResponse()
        response_data = json.loads(response.get_data(as_text=True))
        self.assertEqual(response_data["123"], "value")

    def test_getResponse_none_values(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {"key1": None, "key2": "value"}
        self.handler.setResponse(mock_resp)
        response = self.handler.getResponse()
        response_data = json.loads(response.get_data(as_text=True))
        self.assertIsNone(response_data["key1"])
        self.assertEqual(response_data["key2"], "value")

class TestApirespBase(unittest.TestCase):
    def test_setResponse(self):
        base = apir.ApirespBase()
        with self.assertRaises(NotImplementedError):
            base.setResponse()

    def test_getResponse(self):
        base = apir.ApirespBase()
        with self.assertRaises(NotImplementedError):
            base.getResponse()

if __name__ == '__main__':
    unittest.main()