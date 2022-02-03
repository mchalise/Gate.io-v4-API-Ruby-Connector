# Gate.io-v4-API-Ruby-Connector
Gate.io V4 API Connection Implementation


```ruby
    #
    # path = '/wallet/total_balances'
    # query_string = 'from=1634213866&to=1635596266'
    # data (HASH) {api_key: '', api_secret: ''}
    #
    def self.call_gate_api(path = '', query_string = '', data)
      base_url = "https://api.gateio.ws"
      prefix = "/api/v4"
      full_url =  base_url + prefix + path
      timestamp = (Time.now.utc.to_f).to_i
      request_url = prefix + path
      encoded_payload = Digest::SHA512.hexdigest("")
      params = "GET\n#{request_url}\n#{query_string}\n#{encoded_payload}\n#{timestamp}"
      signature = OpenSSL::HMAC.hexdigest('SHA512', data['api_secret'].encode('utf-8'), params)
      headers = {
        'KEY': data['api_key'].encode('utf-8'),
        'SIGN': signature,
        'Timestamp': Time.now.to_i.to_s,
        'Accept': 'application/json', 
        'Content-Type': 'application/json'
      }
      main_url = full_url+"?#{query_string}"
      resp = HTTParty.get(main_url, headers: headers, format: :json)
      JSON.parse(resp.body, symbolize_names: true)
    end
 ```
