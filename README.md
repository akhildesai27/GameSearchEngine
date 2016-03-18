# GameSearchEngine

from bs4 import BeautifulSoup
import urllib.request
import csv
import MySQLdb, os, sys


class gameSearchEngine:
    
    def __init(self):
        self.fecthData()

    def fecthData(self):
        
        gamePageLinkA = 'http://www.ign.com/games/xbox-360?sortBy=title&sortOrder=asc&letter=A'
        
        if gamePageLinkA == gamePageLinkA:
            gameFile = urllib.request.urlopen(gamePageLinkA)
            gameHtml = gameFile.read()
            gameFile.close()
            soup = BeautifulSoup(gameHtml)
            gameAll = soup.find_all("a")
            for links in soup.find_all('a'):
                try:
                    if links.get('href') == None:
                        pass
                    else:
                        value = (links.get('href'))
                        test = (value.split('/')[2])
                        list = str(test) 
                        stringValues = list
                        myList = stringValues.split(',')
                        for i in myList:
                            try:
                                if i[0:3] == 'www':
                                    pass
                                elif i[-4:-1] == '.co':
                                    pass
                                elif i[0:4] == 'xbox':
                                    pass
                                elif i == None:
                                    pass
                                elif i == 'upcoming':
                                    pass
                                elif i == 'reviews':
                                    pass
                                else:
                                    title =str(i)
                                    self.pubGrid()
                            except Exception as e:
                                print(e)
    
                except Exception as e:
                    print(e)
                    
    def pubGrid(self):
        
        try:
            gameFile = urllib.request.urlopen('http://www.ign.com/games/xbox-360?sortBy=title&sortOrder=asc&letter=W')
            gameHtml = gameFile.read()
            gameFile.close()
            soup = BeautifulSoup(gameHtml)
            gameAll = soup.find_all('div',class_='publisher grid_3' )
           
            for gamesAll in gameAll:
                if gamesAll.string == None:
                    pass
    
                else:
                    pubGrid = gamesAll.string.strip()
                    self.getDataFromWebPage()
                    #print(gamesAll.string.strip())
        except Exception as e:
            print(e)
            
    def getDataFromWebPage(self):
        
        try:
            gameFile = urllib.request.urlopen('http://www.ign.com/games/xbox-360?sortBy=title&sortOrder=asc&letter=W')
            gameHtml = gameFile.read()
            gameFile.close()
            soup = BeautifulSoup(gameHtml)
            gameAll = soup.find_all('div',class_='grid_3' )
           
            for gamesAll in gameAll:
                if gamesAll.string == None:
                    pass
                elif gamesAll.string.strip() == '2K Games':
                    pass
                elif gamesAll.string.strip() == '2K Sports':
                    pass
                elif gamesAll.string.strip() == '505 Games':
                    pass
                elif gamesAll.string.strip() == '345 Games':
                    pass
                elif gamesAll.string.strip().split(',')[0] == 'NR' or gamesAll.string.strip().split(',')[0][0:1] == '0'or gamesAll.string.strip().split(',')[0][0:1] == '1' or gamesAll.string.strip().split(',')[0][0:1] == '2' or gamesAll.string.strip().split(',')[0][0:1] == '3' or gamesAll.string.strip().split(',')[0][0:1] == '4' or gamesAll.string.strip().split(',')[0][0:1] == '5' or gamesAll.string.strip().split(',')[0][0:1] == '6' or gamesAll.string.strip().split(',')[0][0:1] == '7' or gamesAll.string.strip().split(',')[0][0:1] == '8' or gamesAll.string.strip().split(',')[0][0:1] == '9':
                    gameRate = gamesAll.string.strip()
                    self.pubGrid()
                    print(gameRate)
    
                else:
                    pass
        except Exception as e:
            print(e)
            
    def pubGrid(self):
        
        try:
            gameFile = urllib.request.urlopen('http://www.ign.com/games/xbox-360?sortBy=title&sortOrder=asc&letter=W')
            gameHtml = gameFile.read()
            gameFile.close()
            soup = BeautifulSoup(gameHtml)
            gameAll = soup.find_all('div',class_='releaseDate grid_3 omega' )
           
            for gamesAll in gameAll:
                if gamesAll.string == None:
                    pass
    
                else:
                    pubGrid = gamesAll.string.strip()
                    print(gamesAll.string.strip())
                    self.updateDB()
        except Exception as e:
            print(e)
    
    def updateDB(self):
        
        workbook = openpyxl.load_workbook(filename = 'G:/Work/531.xlsx', use_iterators = True)
        worksheet = workbook.get_sheet_by_name('Sheet1')
        for row in worksheet.iter_rows():
            data = {
                'my_first_col':  row[0].value,
                'my_sec_col':  row[1].value,
                'my_third_col':  row[2].value,
                'my_fourth_col':  row[3].value,
                'my_fifth_col':  row[4].value # Column A
            }
            print(data['my_first_col'],data['my_sec_col'],data['my_third_col'],data['my_fourth_col'],data['my_fifth_col'])
            catId = data['my_first_col']
            gN= data['my_sec_col']
            pubD = data['my_third_col']
            rat = data['my_fourth_col']
            relD = data['my_fifth_col']
            try:
                conn = MySQLdb.connect (host = "localhost", user = "root", passwd = "", db = "test1")
                cursor = conn.cursor ()
                quertest = "INSERT INTO gameList (categoryId, gameName, publisher, ratings, releaseDate)  VALUES ('"+str(catId)+"','"+str(gN)+"','"+str(pubD)+"','"+str(rat)+"','"+str(relD)+"' )"
                print(quertest)
                cursor.execute (quertest) 
                row = cursor.fetchone()
            except Exception as e:
                print(e)
            finally:
                self.getGenres()
                conn.commit()
                cursor.close ()
                conn.close ()
    
    def getGenres(self):
        
        gameFile = urllib.request.urlopen('http://www.neoseeker.com/xbox360/xbox-live-arcade/')
        gameHtml = gameFile.read()
        gameFile.close()
        soup = BeautifulSoup(gameHtml)
        gameAll = soup.find_all('a',href='popular' )
        print(gameAll)
        for gameAll in gameAll:
            if gameAll.string == None:
                pass
            else:
                try:
                    gameName = gameAll.string
                    conn = MySQLdb.connect (host = "localhost", user = "root", passwd = "", db = "test1")
                    cursor = conn.cursor ()
                    quertest = "INSERT INTO gameListGenres (gameName, gameGenre)  VALUES ('"+str(gameName)+"', 'Xbox Live Arcade')"
                    print(quertest)
                    cursor.execute(quertest) 
                    row = cursor.fetchone()
                except Exception as e:
                    print(e)
                finally:
                    self.randGamePrice()
                    conn.commit()
                    cursor.close ()
                    conn.close ()
    
    def randGamePrice(self):
        my_randoms = random.sample(range(1570), 1568)
        for p in my_randoms:
            price = (str(p))
            try:
                conn = MySQLdb.connect (host = "localhost", user = "root", passwd = "", db = "test1")
                cursor = conn.cursor ()
                quertest = "INSERT INTO gameprice (gamePrice)  VALUES ('"+price+"' )"
                print(quertest)
                cursor.execute (quertest) 
                row = cursor.fetchone()
            except Exception as e:
                print(e)
            finally:
                conn.commit()
                cursor.close ()
                conn.close ()
    
