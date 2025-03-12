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
        # Test data container for better maintainability
        test_data = {
            'authenticated': False,
            'auth_url': "http://example.com",
            'account': "account1",
            'parms': {"key": "value"},
            'dbsess_obj': "dbsess",
            'newsess_obj': "newsess",
            'dbopsobj': "dbops",
            'status': "active"
        }
        
        authreq = Authreq(**test_data)
        self.assertFalse(authreq.authenticated)
        self.assertEqual(authreq.auth_url, test_data['auth_url'])
        self.assertEqual(authreq.account, test_data['account'])
        self.assertEqual(authreq.parms, test_data['parms'])
        self.assertEqual(authreq.dbsess_obj, test_data['dbsess_obj'])
        self.assertEqual(authreq.newsess_obj, test_data['newsess_obj'])
        self.assertEqual(authreq.dbopsobj, test_data['dbopsobj'])
        self.assertEqual(authreq.status, test_data['status'])

class TestApihomeResp(unittest.TestCase):
    def test_default_initialization(self):
        resp = ApihomeResp()
        self.assertIsNone(resp.useremailid)
        self.assertIsNone(resp.username)
        self.assertIsNone(resp.userguid)
        self.assertIsNone(resp.channelname)
        self.assertIsNone(resp.channelurl)
        self.assertIsNone(resp.sharepointurl)
        self.assertFalse(resp.isAuthorized)
        self.assertIsNone(resp.folder)
        self.assertIsNone(resp.exceptionFolder)
        self.assertFalse(resp.isCopycomplete)
        self.assertIsNone(resp.status)
        self.assertIsNone(resp.message)

    def test_custom_initialization(self):
        # Consolidated test data
        test_data = {
            'useremailid': "user@example.com",
            'username': "user1",
            'userguid': "guid123",
            'channelname': "channel1",
            'channelurl': "http://channel.com",
            'sharepointurl': "http://sharepoint.com",
            'isAuthorized': True,
            'folder': "folder1",
            'exceptionFolder': "exceptionFolder1",
            'isCopycomplete': True,
            'status': "active",
            'message': "success"
        }
        
        resp = ApihomeResp(**test_data)
        self.assertEqual(resp.useremailid, test_data['useremailid'])
        self.assertEqual(resp.username, test_data['username'])
        self.assertEqual(resp.userguid, test_data['userguid'])
        self.assertEqual(resp.channelname, test_data['channelname'])
        self.assertEqual(resp.channelurl, test_data['channelurl'])
        self.assertEqual(resp.sharepointurl, test_data['sharepointurl'])
        self.assertTrue(resp.isAuthorized)
        self.assertEqual(resp.folder, test_data['folder'])
        self.assertEqual(resp.exceptionFolder, test_data['exceptionFolder'])
        self.assertTrue(resp.isCopycomplete)
        self.assertEqual(resp.status, test_data['status'])
        self.assertEqual(resp.message, test_data['message'])

if __name__ == '__main__':
    unittest.main()