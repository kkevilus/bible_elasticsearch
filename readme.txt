Step 1 - Create the index with mapping:

        This PUT request will create an index /bible/ with a document type of 'vs'.
        The fields it accepts are:
             "bk" (book of the bible),
             "chpt" (chapter of the book),
             "vs" (verse of the chapter),
             "text" (text of the verse)


curl -X PUT \
  http://localhost:9200/bible/ \
  -H 'Cache-Control: no-cache' \
  -H 'Content-Type: application/json' \
  -d '{
            "vs": {
                "properties": {
                    "bk": {
                        "type": "text",
                        "fields": {
                            "keyword": {
                                "type": "keyword"
                            }
                        }
                    },
                    "chpt": {
                        "type": "long"
                    },
                    "text": {
                        "type": "text",
                        "fields": {
                            "keyword": {
                                "type": "keyword"
                            }
                        }
                    },
                    "vs": {
                        "type": "long"
                    }
                }
            }
        }'


Populating the data:

curl -X POST \
  http://localhost:9200/bible/vs/_bulk \
  -H 'Cache-Control: no-cache' \
  -H 'Content-Type: application/x-ndjson' \
  -d '***INSERT DATA FROM BULK FILE HERE***'


All Done - let the queries begin:

GET the verse directly:
curl -X GET \
  http://localhost:9200/bible/vs/JHN_3_16 \
  -H 'Cache-Control: no-cache' \

Resulting response:
{
    "_index": "bible",
    "_type": "vs",
    "_id": "JHN_3_16",
    "_version": 1,
    "found": true,
    "_source": {
        "bk": "JHN",
        "chpt": 3,
        "vs": 16,
        "text": "For God so loved the world that he gave his only-begotten Son, so that all who believe in him may not perish, but may have eternal life."
    }
}


Example Query 1 -
Bring back a chapter:

A couple of quick notes on this:
    This is a must match query -  "bk" must be "JHN"
                                  "chpt" must be 3
    I set the "size" to 176 (the largest number of verses in any chapter in this bible)
    I set the "_source" field to "text" so that it wouldn't return a lot of extra unusable data.

REQUEST:

curl -X POST \
  http://localhost:9200/bible/vs/_search \
  -H 'Cache-Control: no-cache' \
  -H 'Content-Type: application/json' \
  -d '{"query":
    {"bool":
        {"must": [
            {"match":{"bk":"JHN"} },
            {"match": {"chpt":3}}]
        }
    },
    "sort":{ "vs": "ASC"},
    "_source":["text"],
    "size":176
}'


RESULTS:

{
    "took": 8,
    "timed_out": false,
    "_shards": {
        "total": 5,
        "successful": 5,
        "failed": 0
    },
    "hits": {
        "total": 36,
        "max_score": null,
        "hits": [
            {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_1",
                "_score": null,
                "_source": {
                    "text": "Now there was a man among the Pharisees, named Nicodemus, a leader of the Jews."
                },"sort": [1] },
            {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_2",
                "_score": null,
                "_source": {
                    "text": "He went to Jesus at night, and he said to him: \"Rabbi, we know that you have arrived as a teacher from God. For no one would be able to accomplish these signs, which you accomplish, unless God were with him.\""
                },"sort": [2] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_3",
                "_score": null,
                "_source": {
                    "text": "Jesus responded and said to him, \"Amen, amen, I say to you, unless one has been reborn anew, he is not able to see the kingdom of God.\""
                },"sort": [3] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_4",
                "_score": null,
                "_source": {
                    "text": "Nicodemus said to him: \"How could a man be born when he is old? Surely, he cannot enter a second time into his mother's womb to be reborn?\""
                },"sort": [4] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_5",
                "_score": null,
                "_source": {
                    "text": "Jesus responded: \"Amen, amen, I say to you, unless one has been reborn by water and the Holy Spirit, he is not able to enter into the kingdom of God."
                },"sort": [5] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_6",
                "_score": null,
                "_source": {
                    "text": "What is born of the flesh is flesh, and what is born of the Spirit is spirit."
                },"sort": [6] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_7",
                "_score": null,
                "_source": {
                    "text": "You should not be amazed that I said to you: You must be born anew."
                },"sort": [7] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_8",
                "_score": null,
                "_source": {
                    "text": "The Spirit inspires where he wills. And you hear his voice, but you do not know where he comes from, or where he is going. So it is with all who are born of the Spirit.\""
                },"sort": [8] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_9",
                "_score": null,
                "_source": {
                    "text": "Nicodemus responded and said to him, \"How are these things able to be accomplished?\""
                },"sort": [9] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_10",
                "_score": null,
                "_source": {
                    "text": "Jesus responded and said to him: \"You are a teacher in Israel, and you are ignorant of these things?"
                },"sort": [10] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_11",
                "_score": null,
                "_source": {
                    "text": "Amen, amen, I say to you, that we speak about what we know, and we testify about what we have seen. But you do not accept our testimony."
                },"sort": [11] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_12",
                "_score": null,
                "_source": {
                    "text": "If I have spoken to you about earthly things, and you have not believed, then how will you believe, if I will speak to you about heavenly things?"
                },"sort": [12] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_13",
                "_score": null,
                "_source": {
                    "text": "And no one has ascended to heaven, except the one who descended from heaven: the Son of man who is in heaven."
                },"sort": [13] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_14",
                "_score": null,
                "_source": {
                    "text": "And just as Moses lifted up the serpent in the desert, so also must the Son of man be lifted up,"
                },"sort": [14] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_15",
                "_score": null,
                "_source": {
                    "text": "so that whoever believes in him may not perish, but may have eternal life."
                },"sort": [15] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_16",
                "_score": null,
                "_source": {
                    "text": "For God so loved the world that he gave his only-begotten Son, so that all who believe in him may not perish, but may have eternal life."
                },"sort": [16] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_17",
                "_score": null,
                "_source": {
                    "text": "For God did not send his Son into the world, in order to judge the world, but in order that the world may be saved through him."
                },"sort": [17] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_18",
                "_score": null,
                "_source": {
                    "text": "Whoever believes in him is not judged. But whoever does not believe is already judged, because he does not believe in the name of the only-begotten Son of God."
                },"sort": [18] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_19",
                "_score": null,
                "_source": {
                    "text": "And this is the judgment: that the Light has come into the world, and men loved darkness more than light. For their works were evil."
                },"sort": [19] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_20",
                "_score": null,
                "_source": {
                    "text": "For everyone who does evil hates the Light and does not go toward the Light, so that his works may not be corrected."
                },"sort": [20] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_21",
                "_score": null,
                "_source": {
                    "text": "But whoever acts in truth goes toward the Light, so that his works may be manifested, because they have been accomplished in God.\""
                },"sort": [21] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_22",
                "_score": null,
                "_source": {
                    "text": "After these things, Jesus and his disciples went into the land of Judea. And he was living there with them and baptizing."
                },"sort": [22] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_23",
                "_score": null,
                "_source": {
                    "text": "Now John was also baptizing, at Aenon near Salim, because there was much water in that place. And they were arriving and being baptized."
                },"sort": [23] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_24",
                "_score": null,
                "_source": {
                    "text": "For John had not yet been cast into prison."
                },"sort": [24] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_25",
                "_score": null,
                "_source": {
                    "text": "Then a dispute occurred between the disciples of John and the Jews, about purification."
                },"sort": [25] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_26",
                "_score": null,
                "_source": {
                    "text": "And they went to John and said to him: \"Rabbi, the one who was with you across the Jordan, about whom you offered testimony: behold, he is baptizing and everyone is going to him.\""
                },"sort": [26] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_27",
                "_score": null,
                "_source": {
                    "text": "John responded and said: \"A man is not able to receive anything, unless it has been given to him from heaven."
                },"sort": [27] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_28",
                "_score": null,
                "_source": {
                    "text": "You yourselves offer testimony for me that I said, 'I am not the Christ,' but that I have been sent before him."
                },"sort": [28] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_29",
                "_score": null,
                "_source": {
                    "text": "He who holds the bride is the groom. But the friend of the groom, who stands and listens to Him, rejoices joyfully at the voice of the groom. And so, this, my joy, has been fulfilled."
                },"sort": [29] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_30",
                "_score": null,
                "_source": {
                    "text": "He must increase, while I must decrease."
                },"sort": [30] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_31",
                "_score": null,
                "_source": {
                    "text": "He who comes from above, is above everything. He who is from below, is of the earth, and he speaks about the earth. He who comes from heaven is above everything."
                },"sort": [31] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_32",
                "_score": null,
                "_source": {
                    "text": "And what he has seen and heard, about this he testifies. And no one accepts his testimony."
                },"sort": [32] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_33",
                "_score": null,
                "_source": {
                    "text": "Whoever has accepted his testimony has certified that God is truthful."
                },"sort": [33] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_34",
                "_score": null,
                "_source": {
                    "text": "For he whom God has sent speaks the words of God. For God does not give the Spirit by measure."
                },"sort": [34] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_35",
                "_score": null,
                "_source": {
                    "text": "The Father loves the Son, and he has given everything into his hand."
                },"sort": [35] },
             {
                "_index": "bible",
                "_type": "vs",
                "_id": "JHN_3_36",
                "_score": null,
                "_source": {
                    "text": "Whoever believes in the Son has eternal life. But whoever is unbelieving toward the Son shall not see life; instead the wrath of God remains upon him.\""
                },
                "sort": [
                    36
                ]
            }
        ]
    }
}



Example Query 2 -

Just searching for the term 'love'.

Note it only returns 10 (Elastic Default) unless you add the size variable-
In this response it found all references to 'love' in this version of the bible in 2ms.

REQUEST:

curl -X GET \
  'http://localhost:9200/bible/vs/_search?q=love' \
  -H 'Cache-Control: no-cache' \

RESULTS:

{
    "took": 2,
    "timed_out": false,
    "_shards": {
        "total": 5,
        "successful": 5,
        "failed": 0
    },
    "hits": {
        "total": 265,
        "max_score": 8.333345,
        "hits": [
            {
                "_index": "bible",
                "_type": "vs",
                "_id": "LUK_6_32",
                "_score": 8.333345,
                "_source": {
                    "bk": "LUK",
                    "chpt": 6,
                    "vs": 32,
                    "text": "And if you love those who love you, what credit is due to you? For even sinners love those who love them."
            } },
            {
                "_index": "bible",
                "_type": "vs",
                "_id": "1JN_4_18",
                "_score": 7.746,
                "_source": {
                    "bk": "1JN",
                    "chpt": 4,
                    "vs": 18,
                    "text": "Fear is not in love. Instead, perfect love casts out fear, for fear pertains to punishment. And whoever fears is not perfected in love."
            } },
            {
                "_index": "bible",
                "_type": "vs",
                "_id": "2PE_1_7",
                "_score": 7.722271,
                "_source": {
                    "bk": "2PE",
                    "chpt": 1,
                    "vs": 7,
                    "text": "and in piety, love of brotherhood; and in love of brotherhood, charity."
            } },
            {   "_index": "bible",
                "_type": "vs",
                "_id": "1JN_4_8",
                "_score": 7.5995793,
                "_source": {
                    "bk": "1JN",
                    "chpt": 4,
                    "vs": 8,
                    "text": "Whoever does not love, does not know God. For God is love."
            } },
            {   "_index": "bible",
                "_type": "vs",
                "_id": "ROM_13_10",
                "_score": 7.3259006,
                "_source": {
                    "bk": "ROM",
                    "chpt": 13,
                    "vs": 10,
                    "text": "The love of neighbor does no harm. Therefore, love is the plenitude of the law."
            } },
            {   "_index": "bible",
                "_type": "vs",
                "_id": "1JN_4_16",
                "_score": 6.973838,
                "_source": {
                    "bk": "1JN",
                    "chpt": 4,
                    "vs": 16,
                    "text": "And we have known and believed the love that God has for us. God is love. And he who abides in love, abides in God, and God in him."
            } },
            {   "_index": "bible",
                "_type": "vs",
                "_id": "1JN_3_14",
                "_score": 6.9056606,
                "_source": {
                    "bk": "1JN",
                    "chpt": 3,
                    "vs": 14,
                    "text": "We know that we have passed from death to life. For we love as brothers. Whoever does not love, abides in death."
            } },
            {   "_index": "bible",
                "_type": "vs",
                "_id": "1JN_4_12",
                "_score": 6.9056606,
                "_source": {
                    "bk": "1JN",
                    "chpt": 4,
                    "vs": 12,
                    "text": "No one has ever seen God. But if we love one another, God abides in us, and his love is perfected in us."
            } },
            {   "_index": "bible",
                "_type": "vs",
                "_id": "1JN_5_2",
                "_score": 6.9056606,
                "_source": {
                    "bk": "1JN",
                    "chpt": 5,
                    "vs": 2,
                    "text": "In this way, we know that we love those born of God: when we love God and do his commandments."
             } },
            {  "_index": "bible",
                "_type": "vs",
                "_id": "PRO_8_17",
                "_score": 6.872608,
                "_source": {
                    "bk": "PRO",
                    "chpt": 8,
                    "vs": 17,
                    "text": "I love those who love me. And those who stand watch for me until morning shall discover me."
                } } ] } }


Example Query 3 - URI Query

The 'love' query above had 265 results.
If we searched 'love+peace' we get 689 results.
Using the 'default_operator=AND' (OR is default)
We limit the results to verses that have both 'love' and 'peace' included.
9 verses are found in this query for both terms.

REQUEST:

curl -X GET \
  'http://localhost:9200/bible/vs/_search?q=love+peace&default_operator=AND' \

RESULTS:

{"took":6,"timed_out":false,"_shards":{"total":5,"successful":5,"failed":0},"hits":{"total":689,"max_score":11.442558,"hits":[{"_index":"bible","_type":"vs","_id":"TOB_13_18","_score":11.442558,"_source":{"bk":"TOB","chpt":13,"vs":18,"text":"Blessed are all those who love you and who rejoice in your peace."}},{"_index":"bible","_type":"vs","_id":"JUD_1_2","_score":11.241892,"_source":{"bk":"JUD","chpt":1,"vs":2,"text":"May mercy, and peace, and love be fulfilled in you."}},{"_index":"bible","_type":"vs","_id":"PS_118_165","_score":10.410945,"_source":{"bk":"PS","chpt":118,"vs":165,"text":"Those who love your law have great peace, and there is no scandal for them."}},{"_index":"bible","_type":"vs","_id":"2CO_13_11","_score":9.469713,"_source":{"bk":"2CO","chpt":13,"vs":11,"text":"As to the rest, brothers, rejoice, be perfect, be encouraged, have the same mind, have peace. And so the God of peace and love will be with you."}},{"_index":"bible","_type":"vs","_id":"PS_121_6","_score":9.41785,"_source":{"bk":"PS","chpt":121,"vs":6,"text":"Petition for the things that are for the peace of Jerusalem, and for abundance for those who love you."}},{"_index":"bible","_type":"vs","_id":"ECC_3_8","_score":9.295891,"_source":{"bk":"ECC","chpt":3,"vs":8,"text":"A time of love, and a time of hatred. A time of war, and a time of peace."}},{"_index":"bible","_type":"vs","_id":"LUK_6_32","_score":8.333345,"_source":{"bk":"LUK","chpt":6,"vs":32,"text":"And if you love those who love you, what credit is due to you? For even sinners love those who love them."}},{"_index":"bible","_type":"vs","_id":"2JN_1_3","_score":7.9950733,"_source":{"bk":"2JN","chpt":1,"vs":3,"text":"May grace, mercy, and peace be with you from God the Father, and from Christ Jesus, the Son of the Father, in truth and in love."}},{"_index":"bible","_type":"vs","_id":"WIS_3_9","_score":7.864598,"_source":{"bk":"WIS","chpt":3,"vs":9,"text":"Those who trust in him, will understand the truth, and those who are faithful in love will rest in him, because grace and peace is for his elect."}},{"_index":"bible","_type":"vs","_id":"1JN_4_18","_score":7.746,"_source":{"bk":"1JN","chpt":4,"vs":18,"text":"Fear is not in love. Instead, perfect love casts out fear, for fear pertains to punishment. And whoever fears is not perfected in love."}}]}}
