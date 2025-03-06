from abc import ABC, abstractmethod
from flask import jsonify

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
