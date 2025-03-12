import unittest
from Entities.Customentities import Authreq, ApihomeResp

class TestAuthreq(unittest.TestCase):

    def test_default_initialization(self):
        authreq = Authreq()
        self.assertTrue(authreq.authenticated)
        self.assertIsNone(authreq.auth_url)
        self.assertIsNone(authreq.account)
        self.assertIsNone(authreq.parms)
        self.assertIsNone(authreq.dbsess_obj)
        self.assertIsNone(authreq.newsess_obj)
        self.assertIsNone(authreq.dbopsobj)
        self.assertIsNone(authreq.status)

    def test_custom_initialization(self):
        authreq = Authreq(authenticated=False, auth_url="http://example.com", account="account1", parms={"key": "value"},
                          dbsess_obj="dbsess", newsess_obj="newsess", dbopsobj="dbops", status="active")
        self.assertFalse(authreq.authenticated)
        self.assertEqual(authreq.auth_url, "http://example.com")
        self.assertEqual(authreq.account, "account1")
        self.assertEqual(authreq.parms, {"key": "value"})
        self.assertEqual(authreq.dbsess_obj, "dbsess")
        self.assertEqual(authreq.newsess_obj, "newsess")
        self.assertEqual(authreq.dbopsobj, "dbops")
        self.assertEqual(authreq.status, "active")

class TestApihomeResp(unittest.TestCase):

    def test_default_initialization(self):
        apihome_resp = ApihomeResp()
        self.assertIsNone(apihome_resp.useremailid)
        self.assertIsNone(apihome_resp.username)
        self.assertIsNone(apihome_resp.userguid)
        self.assertIsNone(apihome_resp.channelname)
        self.assertIsNone(apihome_resp.channelurl)
        self.assertIsNone(apihome_resp.sharepointurl)
        self.assertFalse(apihome_resp.isAuthorized)
        self.assertIsNone(apihome_resp.folder)
        self.assertIsNone(apihome_resp.exceptionFolder)
        self.assertFalse(apihome_resp.isCopycomplete)
        self.assertIsNone(apihome_resp.status)
        self.assertIsNone(apihome_resp.message)

    def test_custom_initialization(self):
        apihome_resp = ApihomeResp(useremailid="user@example.com", username="user1", userguid="guid123", channelname="channel1",
                                   channelurl="http://channel.com", sharepointurl="http://sharepoint.com", isAuthorized=True,
                                   folder="folder1", exceptionFolder="exceptionFolder1", isCopycomplete=True, status="active", message="success")
        self.assertEqual(apihome_resp.useremailid, "user@example.com")
        self.assertEqual(apihome_resp.username, "user1")
        self.assertEqual(apihome_resp.userguid, "guid123")
        self.assertEqual(apihome_resp.channelname, "channel1")
        self.assertEqual(apihome_resp.channelurl, "http://channel.com")
        self.assertEqual(apihome_resp.sharepointurl, "http://sharepoint.com")
        self.assertTrue(apihome_resp.isAuthorized)
        self.assertEqual(apihome_resp.folder, "folder1")
        self.assertEqual(apihome_resp.exceptionFolder, "exceptionFolder1")
        self.assertTrue(apihome_resp.isCopycomplete)
        self.assertEqual(apihome_resp.status, "active")
        self.assertEqual(apihome_resp.message, "success")

if __name__ == '__main__':
    unittest.main()
