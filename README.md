# What is this repo about?
This is a telegram bot writen in python for searching files in Drive.

# How to deploy?

- Clone this repo:
```
git clone https://github.com/SVR666/SearchX-bot search-bot/
cd search-bot
```

### Install requirements

- For Debian based distros
```
sudo apt install python3
sudo snap install docker 
```
- For Arch and it's derivatives:
```
sudo pacman -S docker python
```

## Setting up config file
```
cp config_sample.env config.env
```
- Remove the first line saying:
```
_____REMOVE_THIS_LINE_____=True
```
Fill up rest of the fields. Meaning of each fields are discussed below:
- **BOT_TOKEN** : The telegram bot token that you get from @BotFather
- **OWNER_ID** : The Telegram user ID (not username) of the owner of the bot
- **TELEGRAPH_TOKEN** : The token generated by running:
```
python3 telegraph_token.py
```

## Setting up drive_folder file

- The bot is unable to search in sub-directories, but you can specify directories in which you wanna search.
- Add drive/folder name(anything that u likes), drive id/folder id & index url(optional) corresponding to each id.
- If you are adding a folder id and you wish to use index url, then add index url corresponding to that folder.

- Run driveid.py and follow the screen.
```
python3 driveid.py
```

## Authorizing Chats

- Add telegram id of chats that u wanna authorize into the authorized_chats.txt file.

## Getting Google OAuth API credential file

- Visit the [Google Cloud Console](https://console.developers.google.com/apis/credentials)
- Go to the OAuth Consent tab, fill it, and save.
- Go to the Credentials tab and click Create Credentials -> OAuth Client ID
- Choose Desktop and Create.
- Use the download button to download your credentials.
- Move that file to the root of search-bot, and rename it to credentials.json
- Visit [Google API page](https://console.developers.google.com/apis/library)
- Search for Drive and enable it if it is disabled
- Finally, run the script to generate token file (token.pickle) for Google Drive:
```
pip install google-api-python-client google-auth-httplib2 google-auth-oauthlib
python3 generate_drive_token.py
```

## Deploying on Heroku

- Install [Heroku cli](https://devcenter.heroku.com/articles/heroku-cli)
- Login into your heroku account with command:
```
heroku login
```
- Create a new heroku app:
```
heroku create appname	
```
- Select This App in your Heroku-cli: 
```
heroku git:remote -a appname
```
- Change Dyno Stack to a Docker Container:
```
heroku stack:set container
```
- Add Private Credentials and Config Stuff:
```
git add -f credentials.json token.pickle config.env heroku.yml
```
- Commit new changes:
```
git commit -m "Added Creds."
```
- Push Code to Heroku:
```
git push heroku master --force
```
- Restart Worker by these commands:
```
heroku ps:scale worker=0
```
```
heroku ps:scale worker=1	 	
```
Heroku-Note: Doing authorizations ( /authorize command ) through telegram wont be permanent as heroku uses ephemeral filesystem. They will be reset on each dyno boot. As a workaround you can:
- Make a file authorized_chats.txt and write the user names and chat_id of you want to authorize, each separated by new line
- Then force add authorized_chats.txt to git and push it to heroku
```
git add authorized_chats.txt -f
git commit -asm "Added hardcoded authorized_chats.txt"
git push heroku heroku:master
```

## Deploying on Server
- Start docker daemon (skip if already running):
```
sudo dockerd
```
- Build Docker image:
```
sudo docker build . -t search-bot
```
- Run the image:
```
sudo docker run search-bot
```
# Credits :

- python-aria-mirror-bot - [lzzy12](https://github.com/lzzy12/python-aria-mirror-bot)
