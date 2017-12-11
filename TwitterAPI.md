
# Analysis

1) The BBC shows the greatest amount of positivity in their tweets, which is to be expected because they are the best news service.

2) CBS is the most negative. I was honestly surprised to find that it wasn't Fox News since most of their news is predicated on fear tactics.

3) The New York Times and Fox News have an overall score that is close to zero, with NYT being slightly positive, and Fox slightly negative. This is what I would expect to find from organizations striving for fairness and accuracy in reporting. 


```python
import apikeys
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn
import tweepy
```


```python
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()
```


```python
consumer_key = apikeys.TWITTER_CONS_KEY
consumer_secret = apikeys.TWITTER_CONS_SECRET
access_token = apikeys.TWITTER_ACC_TOKE
access_token_secret = apikeys.TWITTER_ACC_TOKE_SECRET
```


```python
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth, parser=tweepy.parsers.JSONParser())
```


```python
target_user = ("@BBC", "@CBSNews", "@CNN", "@FoxNews", "@nytimes")
```


```python
User = []
Text = []
Date = []
Compound = []
Positive = []
Negative = []
Neutral = []
for user in target_user:
    for x in range(5):
        public_tweets = api.user_timeline(user, page=x)
        for tweet in public_tweets:
            scores = analyzer.polarity_scores(tweet['text'])
            User.append(user)
            Text.append(tweet['text'])
            Date.append(tweet['created_at'])
            Compound.append(scores['compound'])
            Positive.append(scores['pos'])
            Negative.append(scores['neg'])
            Neutral.append(scores['neu'])

sentiments_pd = pd.DataFrame({"User": User,
                              "Text": Text,
                              "Date": Date,
                              "Compound": Compound,
                              "Positive": Positive,
                              "Negative": Negative,
                              "Neutral": Neutral})
```


```python
sentiments_pd['Date'] = pd.to_datetime(sentiments_pd['Date'])
sentiments_pd.sort_values(by="Date", inplace=True)
sentiments_pd
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Compound</th>
      <th>Date</th>
      <th>Negative</th>
      <th>Neutral</th>
      <th>Positive</th>
      <th>Text</th>
      <th>User</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>99</th>
      <td>0.0000</td>
      <td>2017-12-09 09:30:03</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>Who even needs a calculator anyway? \nVia BBC ...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>98</th>
      <td>0.4588</td>
      <td>2017-12-09 10:00:03</td>
      <td>0.000</td>
      <td>0.880</td>
      <td>0.120</td>
      <td>Mason has used his pocket money to fund random...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>97</th>
      <td>0.0000</td>
      <td>2017-12-09 10:30:05</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>👂🏻📖 @SJ_Watson was an audiology specialist bef...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>96</th>
      <td>0.3400</td>
      <td>2017-12-09 10:44:54</td>
      <td>0.000</td>
      <td>0.870</td>
      <td>0.130</td>
      <td>RT @BBCr4today: If you do one thing today...\n...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>95</th>
      <td>-0.6486</td>
      <td>2017-12-09 10:46:31</td>
      <td>0.185</td>
      <td>0.815</td>
      <td>0.000</td>
      <td>RT @bbcmusic: Nearly seven months later, we lo...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>94</th>
      <td>-0.4404</td>
      <td>2017-12-09 10:59:05</td>
      <td>0.195</td>
      <td>0.805</td>
      <td>0.000</td>
      <td>RT @bbcthree: Lynsey is saving condemned dogs ...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>93</th>
      <td>0.3818</td>
      <td>2017-12-09 11:30:05</td>
      <td>0.100</td>
      <td>0.682</td>
      <td>0.218</td>
      <td>Do you feel pressure when giving Christmas gif...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>92</th>
      <td>0.0000</td>
      <td>2017-12-09 12:00:02</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>Danielle is breaking deaf world records left r...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>91</th>
      <td>-0.6369</td>
      <td>2017-12-09 12:31:04</td>
      <td>0.215</td>
      <td>0.785</td>
      <td>0.000</td>
      <td>Many coastal communities are already battling ...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>90</th>
      <td>0.7845</td>
      <td>2017-12-09 13:00:06</td>
      <td>0.000</td>
      <td>0.699</td>
      <td>0.301</td>
      <td>🔬🌡 Heard of Terrific Scientific, the BBC’s sci...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>89</th>
      <td>-0.4019</td>
      <td>2017-12-09 13:35:04</td>
      <td>0.124</td>
      <td>0.876</td>
      <td>0.000</td>
      <td>Dyslexie is a font that aims to overcome some ...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>88</th>
      <td>0.6369</td>
      <td>2017-12-09 14:10:03</td>
      <td>0.000</td>
      <td>0.704</td>
      <td>0.296</td>
      <td>🎵 Jack Black’s Jumanji theme tune is the best....</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>87</th>
      <td>-0.2500</td>
      <td>2017-12-09 14:30:02</td>
      <td>0.125</td>
      <td>0.875</td>
      <td>0.000</td>
      <td>Could you live with your partner alone in a re...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>86</th>
      <td>0.9517</td>
      <td>2017-12-09 15:00:08</td>
      <td>0.000</td>
      <td>0.393</td>
      <td>0.607</td>
      <td>I'm celebrating natural beauty - 'Instagram is...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>85</th>
      <td>0.0000</td>
      <td>2017-12-09 15:30:07</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>Claire Foy talks about what will be her last o...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>84</th>
      <td>0.0000</td>
      <td>2017-12-09 16:00:06</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>Astronomers have discovered the most distant '...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>83</th>
      <td>0.0000</td>
      <td>2017-12-09 16:30:06</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>Alice is a 26-year-old mum. She's married, she...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>82</th>
      <td>0.0000</td>
      <td>2017-12-09 17:00:06</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>'I know it's not real because I'm surrounded b...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>81</th>
      <td>0.0000</td>
      <td>2017-12-09 17:34:07</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>Tonight, @McInTweet is joined by @JasonManford...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>80</th>
      <td>0.3612</td>
      <td>2017-12-09 18:00:04</td>
      <td>0.000</td>
      <td>0.857</td>
      <td>0.143</td>
      <td>Ever wondered what London looked like 2000 yea...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>79</th>
      <td>0.4215</td>
      <td>2017-12-09 18:30:39</td>
      <td>0.000</td>
      <td>0.851</td>
      <td>0.149</td>
      <td>30 years of Fairytale of New York: 10 true tal...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>78</th>
      <td>0.3818</td>
      <td>2017-12-09 18:33:22</td>
      <td>0.000</td>
      <td>0.885</td>
      <td>0.115</td>
      <td>RT @bbcmusic: ✨@RagNBoneManUK brought all the ...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>77</th>
      <td>0.8655</td>
      <td>2017-12-09 18:33:25</td>
      <td>0.000</td>
      <td>0.640</td>
      <td>0.360</td>
      <td>RT @BBCSpringwatch: Brrr, a chilly one tonight...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>76</th>
      <td>0.4939</td>
      <td>2017-12-09 18:33:32</td>
      <td>0.000</td>
      <td>0.856</td>
      <td>0.144</td>
      <td>RT @BBCEarth: In Cape Verde, @ProjetoBioSal is...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>75</th>
      <td>0.3182</td>
      <td>2017-12-09 18:35:51</td>
      <td>0.092</td>
      <td>0.763</td>
      <td>0.145</td>
      <td>RT @bbcstories: Do our young men lack positive...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>74</th>
      <td>0.0000</td>
      <td>2017-12-09 18:39:10</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>RT @bbcthree: YOU'VE BEEN LIVING A LIE.\nhttps...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>73</th>
      <td>0.8911</td>
      <td>2017-12-09 18:39:17</td>
      <td>0.096</td>
      <td>0.468</td>
      <td>0.436</td>
      <td>RT @BBCWthrWatchers: Ok, so #snow can be a pai...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>72</th>
      <td>0.0000</td>
      <td>2017-12-09 19:00:02</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>Sexually frivolous and morally ambiguous. \nMe...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>71</th>
      <td>0.7783</td>
      <td>2017-12-09 19:30:08</td>
      <td>0.000</td>
      <td>0.756</td>
      <td>0.244</td>
      <td>The history of condoms stretches back around 3...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>70</th>
      <td>-0.3595</td>
      <td>2017-12-09 20:00:03</td>
      <td>0.135</td>
      <td>0.865</td>
      <td>0.000</td>
      <td>Have we got five eight-year-olds robbing house...</td>
      <td>@BBC</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>221</th>
      <td>0.2023</td>
      <td>2017-12-11 04:00:21</td>
      <td>0.000</td>
      <td>0.886</td>
      <td>0.114</td>
      <td>Democratic lawmakers have asked the Treasury D...</td>
      <td>@CNN</td>
    </tr>
    <tr>
      <th>102</th>
      <td>0.5434</td>
      <td>2017-12-11 04:03:05</td>
      <td>0.000</td>
      <td>0.695</td>
      <td>0.305</td>
      <td>A "Christmas Carol" with its own little miracl...</td>
      <td>@CBSNews</td>
    </tr>
    <tr>
      <th>122</th>
      <td>0.5434</td>
      <td>2017-12-11 04:03:05</td>
      <td>0.000</td>
      <td>0.695</td>
      <td>0.305</td>
      <td>A "Christmas Carol" with its own little miracl...</td>
      <td>@CBSNews</td>
    </tr>
    <tr>
      <th>328</th>
      <td>0.0772</td>
      <td>2017-12-11 04:05:01</td>
      <td>0.000</td>
      <td>0.920</td>
      <td>0.080</td>
      <td>.@HeyTammyBruce: "Since [@POTUS] was elected, ...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>308</th>
      <td>0.0772</td>
      <td>2017-12-11 04:05:01</td>
      <td>0.000</td>
      <td>0.920</td>
      <td>0.080</td>
      <td>.@HeyTammyBruce: "Since [@POTUS] was elected, ...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>307</th>
      <td>-0.4588</td>
      <td>2017-12-11 04:07:06</td>
      <td>0.176</td>
      <td>0.824</td>
      <td>0.000</td>
      <td>On @ffweekend, @GovMikeHuckabee responded to @...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>327</th>
      <td>-0.4588</td>
      <td>2017-12-11 04:07:06</td>
      <td>0.176</td>
      <td>0.824</td>
      <td>0.000</td>
      <td>On @ffweekend, @GovMikeHuckabee responded to @...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>326</th>
      <td>0.3818</td>
      <td>2017-12-11 04:12:02</td>
      <td>0.000</td>
      <td>0.860</td>
      <td>0.140</td>
      <td>On @WattersWorld, @PressSec Sarah Sanders slam...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>306</th>
      <td>0.3818</td>
      <td>2017-12-11 04:12:02</td>
      <td>0.000</td>
      <td>0.860</td>
      <td>0.140</td>
      <td>On @WattersWorld, @PressSec Sarah Sanders slam...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>421</th>
      <td>0.0000</td>
      <td>2017-12-11 04:12:09</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>Your cat tattoo can have your actual cat in it...</td>
      <td>@nytimes</td>
    </tr>
    <tr>
      <th>400</th>
      <td>0.0000</td>
      <td>2017-12-11 04:12:09</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>Your cat tattoo can have your actual cat in it...</td>
      <td>@nytimes</td>
    </tr>
    <tr>
      <th>325</th>
      <td>0.5267</td>
      <td>2017-12-11 04:14:01</td>
      <td>0.000</td>
      <td>0.825</td>
      <td>0.175</td>
      <td>.@JesseBWatters: "The @FBI, the Department of ...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>305</th>
      <td>0.5267</td>
      <td>2017-12-11 04:14:01</td>
      <td>0.000</td>
      <td>0.825</td>
      <td>0.175</td>
      <td>.@JesseBWatters: "The @FBI, the Department of ...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>304</th>
      <td>0.6369</td>
      <td>2017-12-11 04:15:02</td>
      <td>0.000</td>
      <td>0.826</td>
      <td>0.174</td>
      <td>In his speech at the opening of the Mississipp...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>324</th>
      <td>0.6369</td>
      <td>2017-12-11 04:15:02</td>
      <td>0.000</td>
      <td>0.826</td>
      <td>0.174</td>
      <td>In his speech at the opening of the Mississipp...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>303</th>
      <td>-0.4019</td>
      <td>2017-12-11 04:17:05</td>
      <td>0.144</td>
      <td>0.856</td>
      <td>0.000</td>
      <td>On @ffweekend, @SheriffClarke slammed Rep. Joh...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>323</th>
      <td>-0.4019</td>
      <td>2017-12-11 04:17:05</td>
      <td>0.144</td>
      <td>0.856</td>
      <td>0.000</td>
      <td>On @ffweekend, @SheriffClarke slammed Rep. Joh...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>200</th>
      <td>0.0000</td>
      <td>2017-12-11 04:17:59</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>"The demolition crew was savage." Beijing forc...</td>
      <td>@CNN</td>
    </tr>
    <tr>
      <th>220</th>
      <td>0.0000</td>
      <td>2017-12-11 04:17:59</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>"The demolition crew was savage." Beijing forc...</td>
      <td>@CNN</td>
    </tr>
    <tr>
      <th>302</th>
      <td>0.0000</td>
      <td>2017-12-11 04:18:02</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>.@StacyOnTheRight: "The GDP is through the roo...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>322</th>
      <td>0.0000</td>
      <td>2017-12-11 04:18:02</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>.@StacyOnTheRight: "The GDP is through the roo...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>121</th>
      <td>-0.6597</td>
      <td>2017-12-11 04:18:06</td>
      <td>0.265</td>
      <td>0.664</td>
      <td>0.071</td>
      <td>Celebrities rally for boy bullied at school af...</td>
      <td>@CBSNews</td>
    </tr>
    <tr>
      <th>101</th>
      <td>-0.6597</td>
      <td>2017-12-11 04:18:06</td>
      <td>0.265</td>
      <td>0.664</td>
      <td>0.071</td>
      <td>Celebrities rally for boy bullied at school af...</td>
      <td>@CBSNews</td>
    </tr>
    <tr>
      <th>301</th>
      <td>0.7430</td>
      <td>2017-12-11 04:22:01</td>
      <td>0.000</td>
      <td>0.730</td>
      <td>0.270</td>
      <td>.@CLewandowski_: "This is what America loves -...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>321</th>
      <td>0.7430</td>
      <td>2017-12-11 04:22:01</td>
      <td>0.000</td>
      <td>0.730</td>
      <td>0.270</td>
      <td>.@CLewandowski_: "This is what America loves -...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>100</th>
      <td>-0.1280</td>
      <td>2017-12-11 04:22:07</td>
      <td>0.077</td>
      <td>0.923</td>
      <td>0.000</td>
      <td>If Roy Moore is elected, Sen. Susan Collins sa...</td>
      <td>@CBSNews</td>
    </tr>
    <tr>
      <th>120</th>
      <td>-0.1280</td>
      <td>2017-12-11 04:22:07</td>
      <td>0.077</td>
      <td>0.923</td>
      <td>0.000</td>
      <td>If Roy Moore is elected, Sen. Susan Collins sa...</td>
      <td>@CBSNews</td>
    </tr>
    <tr>
      <th>300</th>
      <td>0.5859</td>
      <td>2017-12-11 04:25:03</td>
      <td>0.000</td>
      <td>0.743</td>
      <td>0.257</td>
      <td>.@MichelleMalkin on #AlSen: "Roy Moore is goin...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>320</th>
      <td>0.5859</td>
      <td>2017-12-11 04:25:03</td>
      <td>0.000</td>
      <td>0.743</td>
      <td>0.257</td>
      <td>.@MichelleMalkin on #AlSen: "Roy Moore is goin...</td>
      <td>@FoxNews</td>
    </tr>
    <tr>
      <th>420</th>
      <td>0.1027</td>
      <td>2017-12-11 04:26:04</td>
      <td>0.105</td>
      <td>0.773</td>
      <td>0.123</td>
      <td>Confused by all the news about Russia and the ...</td>
      <td>@nytimes</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 7 columns</p>
</div>




```python
for user in target_user:
    series = sentiments_pd.loc[sentiments_pd["User"] == user].filter(items=["Compound"])
    plt.scatter(range(100,0,-1), series, alpha=0.7, edgecolors="black", label=user)
plt.xlim(105,-5)
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0)
plt.title("Sentiment Analysis of Media Tweets 12/10/2017")
plt.xlabel("Tweets Ago")
plt.ylabel("Tweet Polarity")
plt.show()
```


![png](output_8_0.png)



```python
users = sentiments_pd['User'].unique()
overall_sentiment = sentiments_pd['Compound'].groupby(sentiments_pd['User']).mean()
```


```python
Labels = ['BBC', 'CBS', 'NYT', 'CNN', 'FOX']
ypos = np.arange(len(users))
plt.bar(ypos, overall_sentiment.values, label=user, color=['lightblue', 'green', 'red', 'blue', 'yellow'])
plt.title("Overall Media Sentiment based on Twitter 12/10/2017")
plt.ylabel('Tweet Polarity')
plt.xticks(ypos, Labels)
plt.show()
```


![png](output_10_0.png)

