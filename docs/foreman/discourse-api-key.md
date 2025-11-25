# Discourse API Key Generator

This Ruby script generates User API Keys for Discourse instances (such as community.theforeman.org).

## What It Does

The script creates a cryptographic key pair and uses it to securely obtain a
User API Key from a Discourse instance. This key can be used for API
authentication when interacting with Discourse programmatically.

## Prerequisites

```bash
gem install addressable
```

## The Script

```ruby
require 'addressable'
require 'openssl'
require 'base64'
require 'json'

PRIVATE_KEY = OpenSSL::PKey::RSA.new(2048)
PUBLIC_KEY = PRIVATE_KEY.public_key

puts 'What is the target site?'
site = STDIN.gets.chomp

template = Addressable::Template.new("#{site}/user-api-key/new{?query*}")

url = template.expand({
  query: {
    application_name: 'ruby',
    client_id: `hostname`,
    scopes: 'read',
    public_key: PUBLIC_KEY,
    nonce: 1
  }
})

puts "navigate to #{url}."
puts
puts "copy the generated key in here"
puts
puts "press ENTER type end and press ENTER again"
puts

$/ = "end"
encoded_key = STDIN.gets.chomp


private_key = OpenSSL::PKey::RSA.new(PRIVATE_KEY)

user_api_key = JSON.parse(private_key.private_decrypt(Base64.decode64(encoded_key)))

puts "Your User API Key is #{user_api_key['key']}"
```

## Usage

1. Save the script to a file (e.g., `discourse_api_key.rb`)
2. Run the script:

   ```bash
   ruby discourse_api_key.rb
   ```

3. When prompted, enter the target Discourse site URL (e.g., `https://community.theforeman.org`)
4. Navigate to the URL displayed by the script
5. Copy the generated encrypted key from the browser
6. Paste it into the terminal, press ENTER, type `end`, and press ENTER again
7. The script will decrypt and display your User API Key

## How It Works

1. **Key Generation**: Creates an RSA 2048-bit key pair
2. **URL Construction**: Builds a Discourse User API Key request URL with the
   public key
3. **Browser Authentication**: You authenticate in the browser and Discourse
   encrypts the API key with your public key
4. **Decryption**: The script decrypts the key using your private key
5. **Output**: Displays the decrypted User API Key

## Security Notes

- The private key is generated locally and never leaves your machine
- The API key is encrypted by Discourse using your public key
- Only your private key can decrypt the API key
- Default scope is set to `read` - modify the `scopes` parameter for different permissions
