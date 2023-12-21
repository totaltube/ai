*Translation*
```http
POST https://adult.ai.total-tube.com/v1/translate
Accept: application/json
Content-Type: application/json
Authorization: Bearer {api_key}

{{
    "batch": [
        {{"from": "en", "to": "ru", "text": "Text to translate", "tags": ["sometag", "someothertag"]}},
        {{"to": "zh", "text": "Some other text to translate"}}
    ]
}}
```
_Example return_:
```json
{{
    "success": true,
    "value": {{
        "results": [
            "Текст для перевода",
            "还有一些要翻译的文本"
        ],
        "tokens_used": 4
    }}
}}
```
_On error, the return may be like this_:
```json
{{
    "success": false,
    "value": "Infrastructure error"
}}
```
`from` is optional. If not provided, source language will be detected automatically.
`tags` are also optional, can be provided for a more accurate translation.
On return code 429 you should just retry after some time, servers are overloaded. 
You can batch process up to 100 texts.
Languages supported:
```
Russian: ru
Chinese: zh
German: de
Spanish: es
Italian: it
French: fr
Japanese: ja
Indonesian: id
Portuguese: pt
Korean: ko
Gujarati: gu
Turkish: tr
Arabic: ar
Vietnamese: vi
Thai: th
Persian: fa
Hindi: hi
Bengali: bn
Polish: pl
Malay: ms
Dutch: nl
Finnish: fi
Norwegian: nb
Greek: el
Hungarian: hu
Bulgarian: bg
Slovak: sk
Danish: da
Slovenian: sl
Hebrew: he
Czech: cs
Swedish: sv
Serbian: sr
Romanian: ro
```

*Rephrasing*
```http
POST https://adult.ai.total-tube.com/v1/rephrase
Accept: application/json
Content-Type: application/json
Authorization: Bearer {api_key}

{{
    "type": "video",
    "batch": [
        {{"text": "The movie of the year", "tags": ["film", "best", "wonderful"]}},
        {{"text": "Another movie, no tags"}}
    ],
    "temperature": 0.5
}}
```
_Example return_:
```json
{{
    "success": true,
    "value": {{
        "results": [
            "The best film of the year - A Must-See",
            "Untagged video part 2"
        ],
        "tokens_used": 16
    }}
}}
```
The `tags` parameter is optional. The `text` parameter is also optional, but only if `tags` are provided.
The `type` parameter can be set to either `video` or `pics` with default `video`.
The `temperature` parameter can be used to make the rephrases more or less creative. The default value is `0.5`.
Batch processing is available, allowing you to process up to 100 titles at one request.

*Making descriptions*
```http
POST https://adult.ai.total-tube.com/v1/description
Accept: application/json
Content-Type: application/json
Authorization: Bearer {api_key}

{{
    "type": "video",
    "batch": [
        {{"text": "The movie of the year", "tags": ["film", "best", "wonderful"], "words": 20}},
        {{"text": "Just movie, nothing else", "words": 12}}
    ],
    "temperature": 0.5
}}
```
_Example return_:
```json
{{
    "success": true,
    "value": {{
        "results": [
            "Get ready to be blown away by the best film of the year! It's a wonderful masterpiece that will leave you breathless.",
            "The video is not intended to be taken seriously and is simply for entertainment."
        ],
        "tokens_used": 41
    }}
}}
```
The `type` parameter accepts values such as `video`, `page`, `category`, `pics`, `page-pics`, `category-pics`, with the default being `video`.
The `tags` parameter is optional, while the `words` parameter can accept a range from `5` to `350` (for `video` and `pics` maximum is `250` currently), with the default set at `35`.
The temperature parameter can be used to make the descriptions more or less creative. The default value is `0.5`.
Batch requests can be made to process up to `100` items in a single request.

Other request types are similar to those used for rephrasing but have different endpoints:

Use `https://adult.ai.total-tube.com/v1/comment` to generate comments.
Use `https://adult.ai.total-tube.com/v1/fix` to correct errors and adjust casing in the source text (`tags` are not needed in this context).

If you wish to generate multiple versions of descriptions or rephrases for the same title, simply repeat the same row in the batch. However, keep in mind that the results may contain duplicates. It is, therefore, your responsibility to filter out these duplicates.
