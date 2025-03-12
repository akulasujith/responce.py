class Authreq:
  def __init__(self, authenticated=True, auth_url=None,account=None,parms=None,
           dbsess_obj=None,newsess_obj=None,dbopsobj=None,status=None):
    self.authenticated = authenticated
    self.auth_url = auth_url
    self.account = account
    self.parms = parms
    self.dbsess_obj = dbsess_obj
    self.newsess_obj = newsess_obj
    self.dbopsobj = dbopsobj
    self.status = status   

class ApihomeResp:
  def __init__(self, useremailid=None,username=None,userguid=None,channelname=None,channelurl=None, sharepointurl=None,isAuthorized=False,folder=None,exceptionFolder=None,
           isCopycomplete=False,status=None,message=None):
    self.useremailid = useremailid
    self.username = username
    self.userguid = userguid
    self.channelname = channelname
    self.channelurl = channelurl
    self.sharepointurl = sharepointurl
    self.isAuthorized = isAuthorized
    self.folder = folder
    self.exceptionFolder = exceptionFolder
    self.isCopycomplete = isCopycomplete
    self.status = status
    self.message = message
