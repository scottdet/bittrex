from urllib import urlencode
import urllib2, urllib
import hashlib
import json
import time
import hmac

# ------------------------------ #
#       Bittrex Functions        #
# ------------------------------ #

class bittrex(object):
    
    # initialize things
    def __init__(self, key, secret):
        self.key = key
        self.secret = secret
        self.public = ['getmarkets', 'getcurrencies', 'getticker', 'getmarketsummaries', 'getmarketsummary', 'getorderbook', 'getmarkethistory']
        self.market = ['buylimit', 'buymarket', 'selllimit', 'sellmarket', 'cancel', 'getopenorders']
        self.account = ['getbalances', 'getbalance', 'getdepositaddress', 'withdraw', 'getorder', 'getorderhistory', 'getwithdrawalhistory', 'getdeposithistory']
    
    # function to query the Bittrex API
    def query(self, method, values={}):
        if method in self.public:
            url = 'https://bittrex.com/api/v1.1/public/'
        elif method in self.market:
            url = 'https://bittrex.com/api/v1.1/market/'
        elif method in self.account: 
            url = 'https://bittrex.com/api/v1.1/account/'
        else:
            return 'Something went wrong.'
        
        url += method + '?' + urlencode(values)
        
        if method not in self.public:
            url += '&apikey=' + self.key
            url += '&nonce=' + str(int(time.time()))
            signature = hmac.new(self.secret, url, hashlib.sha512).hexdigest()
            headers = {'apisign': signature}
        else:
            headers = {}
        
        req = urllib2.Request(url, headers=headers)
        response = json.loads(urllib2.urlopen(req).read())

        if response["result"]:
            return response["result"]
        else:
            return response["message"]
    
    # get pricing data for specific currency pair
    def get_ticker_bittrex(self, market):
        return self.query('getticker', {'market': market})

    # place limit orders through Bittrex, no market orders or FOK options available
    def buy_limit_bittrex(self, market, quantity, rate):
        return self.query('buylimit', {'market': market, 'quantity': quantity, 'rate': rate})
    
    def sell_limit_bittrex(self, market, quantity, rate):
        return self.query('selllimit', {'market': market, 'quantity': quantity, 'rate': rate})
    
    # helps check if the order has filled
    def get_openorders_bittrex(self, market):
        return self.query('getopenorders', {'market': market})

   # get balances for specific currency pair 
    def get_balance_bittrex(self, currency):
        return self.query('getbalance', {'currency': currency})
