# Welcome to my fork

This is my fork with an added tiny feature.

Now you can have **Privileged_Users** in config.py

The translator will now translate text for **Privileged_Users** even if the text message is written in a language listed in **Ignore_Lang** config.py

This is very useful for the channel owner because it blends 2 useful features solving a communication problem between channel owner and foreign users.

1. The bot translator ignores your native language. So it doesn't spam the chat with useless translations.
2. The channel owner can now communicate freely in his own language and it will be translated to **lang_HomeToOther**, usually being English; while the translator doesn't spam the channel.

# Containerfile

Starting a Podman (or Docker) container is easy and simple.

You should save this text in a Containerfile and replace some sed config data where the placeholders are.

```
FROM python:3.11.10-alpine
RUN apk add build-base python3-dev py3-pip git
RUN pip install async_google_trans_new==1.4.5 gTTS==2.2.4 playsound==1.2.2 deepl-translate==1.2.0 twitchio==2.3.0 emoji==2.2.0

#uncomment this if you want to use sayonari's translator v2.7.4 instead of my fork
#RUN wget https://github.com/sayonari/twitchTransFreeNext/archive/refs/tags/v2.7.4.zip
#RUN unzip v2.7.4.zip
#WORKDIR twitchTransFreeNext-2.7.4

#comment this part if you want to use sayonari's latest code
RUN git clone https://github.com/comandanteciruela/twitchTransFreeNext
WORKDIR twitchTransFreeNext

RUN sed -i 's/^Twitch_Channel.*/Twitch_Channel = "your_twitch_channel_name"/' config.py
RUN sed -i 's/^Trans_Username.*/Trans_Username = "your_translator_bot_name"/' config.py
RUN sed -i 's/^Trans_OAUTH.*/Trans_OAUTH = "this_is_bot_oauth_get_it_in_twitchtokengenerator_dot_com"/' config.py
RUN sed -i 's/^lang_TransToHome.*/lang_TransToHome = "this_is_your_own_language_es_en_or_jp_or_whatever"/' config.py
RUN sed -i 's/^Ignore_Lang.*/Ignore_Lang = ["this_is_also_your_own_language_es_en_or_jpg_or_whatever"]/' config.py

#new config setting for my fork
RUN sed -i 's/^Privileged_Users.*/Privileged_Users = ["your_twitch_channel_name"]/' config.py

CMD python twitchTransFN.py
```

### Build the container image

`$ podman build -t translator_image -f Containerfile`

### Run a container based in the container image you just built

`$ podman run --rm -d translator_image`

I didn't spend too much time messing around with the container to make TTS (text-to-speech) work, so I disabled it via config.py. If anyone knows how to do it, please contact me.

Many thanks to sayonari-san for programming the translator.

---


# twitchTransFreeNext
Next Generation of twitchTransFree!!!!

# Official Webpage
http://www.sayonari.com/trans_asr/

# USAGE
1. rewrite `config.py`
2. double-click `twitchTransFN.exe`

That's all!

NOTE: The file type of config was chaged from .txt to .py!

# I support my wife 24/7 :-) 
This software is made for my wife!  
http://twitch.tv/saatan_pion/  
If you are satisfied by this software, please watch my wife's stream! We are waiting for comming you! and subscribe! donation!

# We welcome your DONATE!!!!
Donation is possible from the following link!  
もし便利だなと思ったら．以下からDONATEしてください．開発中に食べるお菓子代にします！！！  
https://twitch.streamlabs.com/saatan_pion#/

# Please link from your page!
プログラム使うときには，twitchページからリンクを張ってくれたら嬉しいです！（強制ではないです）

さぁたんチャンネルと，翻訳ちゃんのページにリンクを貼っていただけると良いですが，紹介文は各自で考えてくださいρ

[example]  
Twitch: saatan  
http://twitch.tv/saatan_pion/ 

Software: twitchTransFreeNext  
https://github.com/sayonari/twitchTransFreeNext

紹介用の絵も頂いちゃいました．使ってください．  
![trans_anomon](https://user-images.githubusercontent.com/16011609/49361210-c1f5ef80-f71e-11e8-8cff-6fd760e8738a.png)  
Painted by anomon  
https://www.twitch.tv/anomomm

# config.py
```python
######################################################
# PLEASE CHANGE FOLLOWING CONFIGS ####################
Twitch_Channel          = 'target_channel_name'

Trans_Username          = 'trans_user_name'
Trans_OAUTH             = 'oauth_for_trans_user'

#######################################################
# OPTIONAL CONFIGS ####################################
Trans_TextColor         = 'GoldenRod'
# Blue, Coral, DodgerBlue, SpringGreen, YellowGreen, Green, OrangeRed, Red, GoldenRod, HotPink, CadetBlue, SeaGreen, Chocolate, BlueViolet, and Firebrick

lang_TransToHome        = 'ja'
lang_HomeToOther        = 'en'

Show_ByName             = True
Show_ByLang             = True

Ignore_Lang             = ['']
Ignore_Users            = ['Nightbot', 'BikuBikuTest']
Ignore_Line             = ['http', 'BikuBikuTest', '888', '８８８']
Ignore_WWW              = ['w', 'ｗ', 'W', 'Ｗ', 'ww', 'ｗｗ', 'WW', 'ＷＷ', 'www', 'ｗｗｗ', 'WWW', 'ＷＷＷ', '草']
Delete_Words            = ['saatanNooBow', 'BikuBikuTest']

# Any emvironment, set it to `True`, then text will be read by TTS voice!
# TTS_In:User Input Text, TTS_Out:Bot Output Text
TTS_In                  = True
TTS_Out                 = True
TTS_Kind                = "gTTS" # You can choice "CeVIO" if you want to use CeVIO as TTS.
# CeVIO_Cast            = "さとうささら" # When you are using CeVIO, you must set voice cast name.
TTS_TextMaxLength       = 30
TTS_MessageForOmitting  = "以下略"

# if you make TTS for only few lang, please add langID in the list
# for example, ['ja'] means Japanese only, ['ko','en'] means Korean and English are TTS!
ReadOnlyTheseLang       = []

# Select the translate engine ('deepl' or 'google')
Translator              = 'deepl'

# Use Google Apps Script for tlanslating
# e.g.) GAS_URL         = 'https://script.google.com/macros/s/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/exec'
GAS_URL                 = ''

# Enter the suffix of the Google Translate URL you normally use.
# Example: translate.google.co.jp -> 'co.jp'
#          translate.google.com   -> 'com'
GoogleTranslate_suffix  = 'co.jp'

# If you meet any bugs, You can check some error message using Debug mode (Debug = True)
Debug                   = False

```

| Option| Description |
| -- | -- |
| Twitch_Channel | The target chat room for translation. |
| Trans_Username | username for translation |
| Trans_OAUTH | Get key for Trans_Username at https://twitchapps.com/tmi/ |
| Trans_TextColor  | You can change text color of translator. |
| lang_TransToHome | If set it to [`ja`], all texts will be translated to the JAPANESE! |
| lang_HomeToOther | If set it to [`en`], the language in [`lang_TransToHome`] is trans to [`en`]. |
| Show_ByName | If it is set to `True`, user name is shown after translated text. |
| Show_ByLang | If it is set to `True`, the source language is shown after translated text. |
| Ignore_Lang | You can set some languages : [ja,en, ...] |
| Ignore_Users | You can set some users : [Nightbot, BikuBikuTest, someotheruser, ...] |
| Ignore_Line | If the words are in message, the message will be ignored.|
| Ignore_WWW | Ignore Tanshiba(単芝:just only 'w'）line. |
| Delete_Words | The words will be removed from message. |
| TTS_Kind | The kind of TTS, "gTTS"(default) or "CeVIO". If you want to use CeVIO, you need to install CeVIO AI in your local computer. |
| TTS_In | Input text will be read by TTS voice! |
| TTS_Out | Bot output text will be read by TTS voice! |
| gTTS_In | It's deprecated config, please use TTS_In instead. |
| gTTS_Out | It's deprecated config, please use TTS_Out instead. |
| TTS_TextMaxLength | You can specify TTS's read comment max length. If comment has over length from this, it will omit and added TTS_MessageForOmitting on postfix. |
| TTS_MessageForOmitting | A message for omitting TTS's read comment. The TTS puts this message  on the omitted message. |
| CeVIO_Cast | The cast name of CeVIO, for example "さとうささら". This option is enabled only when TTS_Kind = "CeVIO". |
| ReadOnlyTheseLang | You can set the TTS language! |
| Debug | You can check some error message using Debug mode (Debug = True)|


# memo
## support language (google translator)
https://cloud.google.com/translate/docs/languages

# secret functions
## choose trans destination language (for one text)
At the time of translation, you can select the target language like `en:` at the beginning of the sentence.  
Example) ru: Hello -> привет там

NOTE: When rewriting config.txt, please delete the `#` mark at the beginning of each setting value!

## command: (version)
`!ver`: print the software version.

`!sound xxxx`: play sound (xxxx.mp3), if you put sound data at sound folder.

# Thanks
Thanks to Pioneers!
The developer of ...
- Google
- googletrans by ssut
    - https://github.com/ssut/py-googletrans
- gtts by pndurette
    - https://github.com/pndurette/gTTS
- playsound by TaylorSMarks
    - https://github.com/TaylorSMarks/playsound
- TwitchIO
    - https://github.com/TwitchIO/TwitchIO

# Developer Info.

| Title | Automatic Translator for Twitch Chat (Next Generation) |
|--|--|
| Developer | husband_sayonari_omega |
| github | https://github.com/sayonari/twitchTransFreeNext |
| Webpage | http://www.sayonari.com/trans_asr/ |
| mail | sayonari@gmail.com |
| Twitter | [sayonari](https://twitter.com/sayonari) |
