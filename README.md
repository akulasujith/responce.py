import unittest

class TestAuthreq(unittest.TestCase):
    def test_default_initialization(self):
        auth = Authreq()
        self.assertTrue(auth.authenticated)
        self.assertIsNone(auth.auth_url)
        self.assertIsNone(auth.account)
        self.assertIsNone(auth.parms)
        self.assertIsNone(auth.dbsess_obj)
        self.assertIsNone(auth.newsess_obj)
        self.assertIsNone(auth.dbopsobj)
        self.assertIsNone(auth.status)

    def test_all_parameters_initialization(self):
        test_params = {
            'authenticated': False,
            'auth_url': 'https://auth.example.com',
            'account': 'test_account',
            'parms': {'param1': 'value1'},
            'dbsess_obj': 'db_session',
            'newsess_obj': 'new_session',
            'dbopsobj': 'db_operations',
            'status': 404
        }

        auth = Authreq(**test_params)
        self.assertFalse(auth.authenticated)
        self.assertEqual(auth.auth_url, test_params['auth_url'])
        self.assertEqual(auth.account, test_params['account'])
        self.assertEqual(auth.parms, test_params['parms'])
        self.assertEqual(auth.dbsess_obj, test_params['dbsess_obj'])
        self.assertEqual(auth.newsess_obj, test_params['newsess_obj'])
        self.assertEqual(auth.dbopsobj, test_params['dbopsobj'])
        self.assertEqual(auth.status, test_params['status'])

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

    def test_all_parameters_initialization(self):
        test_params = {
            'useremailid': 'user@example.com',
            'username': 'test_user',
            'userguid': '1234-5678',
            'channelname': 'Test Channel',
            'channelurl': 'https://channel.example.com',
            'sharepointurl': 'https://sharepoint.example.com',
            'isAuthorized': True,
            'folder': '/main_folder',
            'exceptionFolder': '/exceptions',
            'isCopycomplete': True,
            'status': 200,
            'message': 'Operation successful'
        }

        resp = ApihomeResp(**test_params)
        self.assertEqual(resp.useremailid, test_params['useremailid'])
        self.assertEqual(resp.username, test_params['username'])
        self.assertEqual(resp.userguid, test_params['userguid'])
        self.assertEqual(resp.channelname, test_params['channelname'])
        self.assertEqual(resp.channelurl, test_params['channelurl'])
        self.assertEqual(resp.sharepointurl, test_params['sharepointurl'])
        self.assertTrue(resp.isAuthorized)
        self.assertEqual(resp.folder, test_params['folder'])
        self.assertEqual(resp.exceptionFolder, test_params['exceptionFolder'])
        self.assertTrue(resp.isCopycomplete)
        self.assertEqual(resp.status, test_params['status'])
        self.assertEqual(resp.message, test_params['message'])

if __name__ == '__main__':
    unittest.main()