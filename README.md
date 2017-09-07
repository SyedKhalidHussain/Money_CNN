# Money_CNN
# Retrieve Stock List from CNN website


import re
import requests
import pandas as pd


def retrieve_stock_list():
    fhandle = requests.get('http://money.cnn.com/data/dow30/')           # Get the file handle of url
    search_pattern = re.compile ( 'class="wsod_symbol">(.*)<\/a>.*<span.*">(.*?)<\/span>.*\n.*class="wsod_stream">(.*)<\/span>')      #Compile the search pattern using regular expression
    stock_list_in_text = re.findall(search_pattern,fhandle.text)        #Return the search pattern in string "fhandle.text"
    stock_list = list()
    for item in stock_list_in_text:              #Go line by line in stock_list_in_text
        stock_list.append( [item[0], item[1], float(item[2])] )             #Append the item indexing 0,1 & 2 in stock_list
    return stock_list

stock_list = retrieve_stock_list()            # Calling the definition of function "retrieve_stock_list"
stock_list_df = pd.DataFrame(stock_list)      # Transfering the stock_list into DataFrame
print(stock_list_df)
