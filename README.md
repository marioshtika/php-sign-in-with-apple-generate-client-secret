Sign in with Apple - Generate the client secret
===============================================

Based on [this](https://developer.okta.com/blog/2019/06/04/what-the-heck-is-sign-in-with-apple) tutorial

General informations
--------------------

Rather than static client secrets, Apple requires that you derive a client secret yourself from your private key. They use the JWT standard for this, using an elliptic curve algorithm with a P-256 curve and SHA256 hash. In other words, they use the ES256 JWT algorithm. Some JWT libraries don’t support elliptic curve methods, so make sure yours does before you start trying this out.

The Ruby JWT library supports this algorithm, so we’ll use that to generate the secret.

This code generates a JWT using the ES256 algorithm which includes a handful of claims. **This JWT expires in 6 months, which is the maximum lifetime Apple will allow.** If you’re generating a new client secret JWT every time a user authenticates, then you should use a much shorter expiration date, but this allows us to generate the secret once and use it in our sample apps easily.

This is described in Apple’s documentation [Generate and validate tokens](https://developer.apple.com/documentation/signinwithapplerestapi/generate_and_validate_tokens).

Preparation
-----------

**Step 1**

First, make sure you’ve got Ruby installed, and then install the JWT gem by running this from the command line:

```
gem install jwt
```

Now the `jwt` gem should be available for use.

**Step 2**

Download the `client-secret.rb` file and fill the missing values at the top of the file.

**Step 3**

Now you can run this from the command line and it will output a JWT.

```
ruby client-secret.rb
```

How to find the needed values
-----------------------------

**key_file**

Rather than using simple strings as OAuth client secrets, Apple has decided to use a public/private key pair, where the client secret is actually a signed JWT.

Go in **Certificates, Identifiers & Profiles** screen, choose **Keys** from the side navigation.

Click the blue plus icon to register a new key. Give your key a name, and check the **Sign In with Apple** checkbox.

Click the **Configure** button and select your primary App ID you created earlier.

Apple will generate a new private key for you and let you download it only once. Make sure you save this file, because you won’t be able to get it back again later! The file you download will end in `.p8`, but it’s just text inside, so rename it to `key.txt` so that it’s easier to use in the next steps.

**team_id**

Find your team ID in the top right corner

**client_id**

The Identifier you entered for your App Id (mobile) or Services ID (web)

**key_id**

Find your key ID in the "Certificates, Identifiers & Profiles" / "Keys" for the previously created key.
