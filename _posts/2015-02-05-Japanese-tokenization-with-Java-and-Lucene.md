---
layout: post
title: "Japanese Tokenization with Java and Lucene"
description: ""
category: Code 
tags: [Java, Lucene, Japanese Tokenization]
---
{% include JB/setup %}
I was trying to write Japanese analysis program with Java and Lucene 4.4. After trying Lucene's CJKAnalyzer and Lucene-gosen, I ended up writing my own Tokenizer, Filter and Analyzer.

# Lucene CJKAnalyzer
Lucene 4.4 comes with a built-in analyzer for Chinese, Japanese and Korean. The demo result for Chinese on [Lucene's document](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/cjk/package-summary.html) seem quite good, so I gave it a try on Japanese:
 
{% highlight java %}
final String s = "バカです。よろしくお願いいたします";
final CJKAnalyzer cjkAnalyzer = new CJKAnalyzer(Version.LUCENE_44);
final TokenStream tokenStream = cjkAnalyzer.tokenStream("", new StringReader(s));
final CharTermAttribute charTermAttribute = tokenStream.addAttribute(CharTermAttribute.class);
tokenStream.reset();
while (tokenStream.incrementToken()) {
    System.out.println(charTermAttribute.toString());
}
{% endhighlight %}
And here's what I got:
```
バカ
カで
です
よろ
ろし
しく
くお
お願
願い
いい
いた
たし
しま
ます
```
Boo - it's pure bigrams of the sentence. Most of the bigrams actaully make no sense in Japanese. Very `バカ` :)
 
# Lucene-gosen
I tried another one that works with Lucene called Lucene-gosen, but taking a look at the [source code](https://github.com/lucene-gosen/lucene-gosen/blob/master/src/java/org/apache/lucene/analysis/gosen/GosenAnalyzer.java), it apparently doesn't work with Lucene 4.4.
 
# Sen
[Sen](https://java.net/projects/sen) seems to be the original project that Lucene-gosen is based on, so I guess we can wrap up our own Tokenizer with Sen's components:

{% highlight java %}
import net.java.sen.StreamTagger;
import net.java.sen.Token;
import org.apache.lucene.analysis.Tokenizer;
import org.apache.lucene.analysis.tokenattributes.CharTermAttribute;
import java.io.IOException;
import java.io.Reader;
 
public final class JapaneseTokenizer extends Tokenizer {
    private final StreamTagger tagger;
 
    private final CharTermAttribute termAttr;
 
    public JapaneseTokenizer(final Reader in, final String senConfPath) throws IOException {
        super(in);
        tagger = new StreamTagger(in, senConfPath);
 
        termAttr = addAttribute(CharTermAttribute.class);
    }
 
    @Override
    public boolean incrementToken() throws IOException {
        if (!tagger.hasNext()) {
            return false;
        }
 
        final Token token = tagger.next();
        termAttr.setEmpty();
        termAttr.append(token.getSurface(), 0, token.length());
        return true;
    }
}
{% endhighlight %}
Apart from Tokenizer, we should also provide some filters to do common Japanese processing tricks like removing punctuations, normalizing half-with characters, and ruling out stopwords, etc. And here's the result I got using the Sen-based Tokenzier:
```
バカ
です
よろしく
お願い
いたし
ます
```
Looks good! Cheers!

**UPDATE**
I was told that [MeCab](https://code.google.com/p/mecab/) is more popular in the Japanese IT industry. I recommend you to try it out if Sen cannot meet your need.