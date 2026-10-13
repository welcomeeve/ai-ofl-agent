# Free AI Language & Document Agent (Starter)

এই starter project-এ একটি সাধারণ ওয়েব ফর্ম এবং Telegram bot আছে। Gemini API দিয়ে লেখা তৈরি করে `.docx` ফাইল পাঠায়।

## ১) যা লাগবে
- GitHub account
- Google AI Studio থেকে Gemini API key
- Telegram-এ BotFather দিয়ে তৈরি bot token
- একটি Python hosting service যেখানে environment variables সেট করা যায়

**খরচ সম্পর্কে:** Gemini API-এর free tier-এ মডেল ও কোটা সীমিত। Hosting service-এর free plan বদলাতে পারে, এবং ঘুমিয়ে গেলে বট সবসময় অনলাইনে নাও থাকতে পারে। GitHub repository নিজে থেকে সারাক্ষণ Python web server চালায় না।

## ২) Gemini API key
Google AI Studio-তে API key তৈরি করুন: https://aistudio.google.com/app/apikey
- key কাউকে পাঠাবেন না।
- GitHub code-এর মধ্যে key লিখবেন না।
- hosting-এর Environment Variables-এ `GEMINI_API_KEY` নামে রাখুন।

Gemini usage/quota: https://ai.google.dev/gemini-api/docs/billing

## ৩) Telegram bot token
Telegram-এ `@BotFather` খুলে `/newbot` দিয়ে bot তৈরি করুন।
- token কপি করুন।
- hosting-এর Environment Variables-এ `TELEGRAM_BOT_TOKEN` নামে রাখুন।
- ঐচ্ছিকভাবে `ALLOWED_TELEGRAM_USER_ID` সেট করে নিজের Telegram numeric user ID-তে সীমাবদ্ধ করুন।
- token কখনো public repository-তে দেবেন না।

## ৪) GitHub-এ আপলোড
1. GitHub-এ New repository তৈরি করুন; Public বা Private যেকোনোটি।
2. এই ZIP extract করে ফাইলগুলো repository-র root-এ upload করুন।
3. `app.py`, `requirements.txt`, `Procfile`, `.gitignore`, `README_BN.md` root-এ আছে নিশ্চিত করুন।

## ৫) Hosting-এ deploy
একটি Python web hosting service-এ repository connect করুন:
- Build/install command: `pip install -r requirements.txt`
- Start command: `gunicorn --workers 1 --threads 4 --timeout 120 app:app`
- Environment variables:
  - `GEMINI_API_KEY` = আপনার Gemini API key
  - `TELEGRAM_BOT_TOKEN` = BotFather token
  - `GEMINI_MODEL` = `gemini-2.5-flash` (যদি আপনার free-tier project-এ model unavailable হয়, AI Studio-তে available model দেখে পরিবর্তন করুন)
  - `ALLOWED_TELEGRAM_USER_ID` = ঐচ্ছিক, আপনার Telegram numeric user ID

**সতর্কতা:** একই token দিয়ে একসঙ্গে একাধিক polling instance চালাবেন না। Free hosting-এ sleep/restart হলে Telegram polling বন্ধ হতে পারে। Production-এ web process ও bot worker আলাদা করা উত্তম, তবে free hosting-এ দুটি process-এর সীমা থাকতে পারে।

## ৬) Telegram ব্যবহার
Bot-এ `/start` পাঠান। তারপর উদাহরণটি কপি করে নিজের তথ্য বসান:

```text
/make
Type: Offer Letter
Language: French
Name: Marie Dupont
Gender: Woman
Company: ABC Solutions
Job title: Operations Manager
Tone: Formal and professional
Details: Start date is 1 December 2026. Include only these facts.
```

`Language:`-এ যে ভাষা চান তার নাম লিখুন। `Gender:` না জানলে `Unknown / not specified` রাখুন। নাম দেখে gender অনুমান করা হবে না।

## ৭) Website ব্যবহার
Deploy-এর URL খুলুন। ফর্মে document type, target language, প্রাপকের তথ্য ও নির্দেশনা লিখে Word ডকুমেন্ট তৈরি করুন।

## নিরাপত্তা ও গুণমান
- অফার লেটার একটি খসড়া; কোম্পানির অনুমোদন/স্বাক্ষর নিজে যাচাই করুন।
- ক্লায়েন্টের passport, bank details বা অপ্রয়োজনীয় sensitive data পাঠাবেন না।
- AI অনুবাদ ভুল করতে পারে; আইনগত বা চাকরিসংক্রান্ত চিঠি native speaker/যোগ্য পেশাজীবী দিয়ে যাচাই করান।
- এই starter app-এ login নেই। ব্যক্তিগতভাবে ব্যবহার করতে hosting access restrict করুন অথবা authentication যোগ করার আগে public URL শেয়ার করবেন না।
- Gemini free tier-এর নীতিতে API data ব্যবহারের শর্ত থাকতে পারে। confidential client documents দেওয়ার আগে বর্তমান terms যাচাই করুন।
