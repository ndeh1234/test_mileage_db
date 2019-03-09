# test_mileage_db
import mileage
import sqlite3
from unittest import TestCase
import unittest

class TestMileageDB(TestCase):

    test_db_url = 'test_miles.db'

    #The name of this method is important - the test runner will look for it

    def setup(self):
        # Overwrite the mileage
        mileage.db_url = set.test_db_url
        # drop everything from the DB to always start with an empty database
        conn = sqlite3.connect(self.test_db_url)
        cursor = conn.cur()
        cursor.execute('DELETE FROM miles')
        conn.commit()
        conn.close()

    def test_add_new_vehicle(self):
        mileage.add_miles('Blue car', 100)
        expected ={'Blue car': 100}

        self.compare_db_to_expected(expected)

        mileage.add_miles('Red Car',50)
        expected['Red Car'] = 100 + 50
        self.compare_db_to_expected(expected)

        # This is not a test method, instead, its used by the test method
        def compare_db_to_expected(self,expected):

            conn = sqlite3.connect(self.test_db_url)
            cursor = conn.cursor()
            all_data = cursor.execute('SELECT * FROM MILES').fetchall()

            # Some rows in DB as entries in expected dictionary
            self.assertEqual(len[expected.keys(), len[all_data]])

            for row in all_data:

                 # vehicle exists, and mileage is correct
                 self.assertIn(row[0], expected.keys())
                 self.assertEqual(expected[row],row[1])

            conn.close()


class SearchVehicle(unittest.TestCase):
 def setup(self, vehicle,new_miles):
     mileage.db_url = set.test_db_url
     conn = sqlite3.connect(self.test_db_url)
     cursor = conn.cur()
     rows_mod = cursor.execute('UPDATE MILES SET total_miles = total_miles + ? WHERE vehicle =?', (new_miles, vehicle))
     if rows_mod.rowcount == 0:
        cursor.execute('INSERT INTO MILES VALUES (?,?)', (vehicle, new_miles))
     conn.commit()
     conn.close()

 def test_search_vehicle(self):
     with self.assertRaises(Exception) as context:
        SearchVehicle('abc',3)
 # Instead of abc give the name of a vehicle which is not present in the database

     self.assertTrue('vehicle not found in context.exception')

''' Instead of abc, give the name of a vehicle which is not found in the database'''


if __name__==' --main--':
        unittest.main()














