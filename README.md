import unittest
from abc import ABC, abstractmethod
from flask import jsonify, Flask

class ApirespBase(ABC):
    def __init__(self):
        self._respjson = None

    @abstractmethod
    def setResponse(self,resp=None):
      raise NotImplementedError

    @abstractmethod
    def getResponse(self):
      raise NotImplementedError

class ApirespHandler(ApirespBase):
    def __init__(self):
        super().__init__()

    def setResponse(self,resp=None):    
        self._respjson = resp

    def getResponse(self):    
        respdict = self._respjson.__dict__
        respdict.pop("channelname",None)
        respdict.pop("channelurl",None)
        return jsonify(respdict)

class MockResponse:
    def __init__(self, data):
        self.__dict__ = data

class TestApirespHandler(unittest.TestCase):
    def setUp(self):
        self.app = Flask(__name__)
        self.app_context = self.app.app_context()
        self.app_context.push()

    def tearDown(self):
        self.app_context.pop()

    def test_set_response(self):
        handler = ApirespHandler()
        mock_data = {"key1": "value1", "key2": "value2"}
        mock_resp = MockResponse(mock_data)
        handler.setResponse(mock_resp)
        self.assertEqual(handler._respjson, mock_resp)

    def test_get_response_without_channel_info(self):
        handler = ApirespHandler()
        mock_data = {"key1": "value1", "key2": "value2"}
        mock_resp = MockResponse(mock_data)
        handler.setResponse(mock_resp)
        response = handler.getResponse()
        self.assertEqual(response.get_json(), mock_data)

    def test_get_response_with_channel_info(self):
        handler = ApirespHandler()
        mock_data = {"key1": "value1", "channelname": "test_channel", "channelurl": "test_url"}
        mock_resp = MockResponse(mock_data)
        handler.setResponse(mock_resp)
        response = handler.getResponse()
        expected_data = {"key1": "value1"}
        self.assertEqual(response.get_json(), expected_data)

    def test_abstract_methods_of_base(self):
        class DummyApirespBase(ApirespBase):
            def setResponse(self, resp=None):
                pass

            def getResponse(self):
                pass

        dummy_instance = DummyApirespBase()
        self.assertTrue(isinstance(dummy_instance,ApirespBase))

if __name__ == '__main__':
    unittest.main(argv=['first-arg-is-ignored'], exit=False)
