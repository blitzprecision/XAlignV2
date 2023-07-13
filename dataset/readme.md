# dataset
It consists of an extensive collection of a high quality cross-lingual fact-to-text dataset in 11 languages: hi, mr, te, ta, en, gu, bn, kn, pa, as, or, ml, and monolingual dataset in English (en).

## Data Format
Each directory contains language specific dataset (refered through language ISO code) and contains of three files:

- train.jsonl
- test.jsonl
- val.jsonl

Data stored in the above files are of JSON Line (jsonl) format, and one can refer [this](https://jsonlines.org/) for more details.

### Record structure (JSON structure)
Each record consist of the following entries:

- entity_name (string) : Title of Wikipedia article from which sentence is extracted.
- qid (string) : Wikidata ID for the given entity.
- sentence (string) : Native language wikipedia sentence. (non-native language strings were removed.) 
- translated_sentence (string) : Automatic English translation of the given sentence. 
- native_sentence_section (string) : Wikipedia section header within which the sentence belongs
- translated_sentence_section (string) : Automatic English translation of Wikipedia section header. 
- sent_index (int) : Offset of the sentence from start in the Wikipedia article.
- `facts` (List[List]) : List of facts associated with the sentence where each fact is list of size four.
- lang (string) : Language identifier.
- sent_score (float) : stage-I predicted score. (for internal use)
- coverage (string) : Automatic method of identifying complete/partial overlap of facts with sentence. (for internal use)
- order (list of indices) : optimal order of facts created based on vanilla mt5 cross-attentions.
- role_spec_order (list of indices) : optimal order of facts created based on mt5_with_role_specific_embeddings cross-attentions.

The `facts` key contains list of facts where each facts is stored as list. A single fact within fact list contains following entries at respective indexes (starts with 1):

1. predicate (string) : property of the fact
2. object (string) : value of the fact for a given predicate.
3. qualifiers (List[List]) : List of qualifier where each qualifier is a list of size two. The first entry is qualifier predicate and second entry is qualifier object. 
4. backward facts (boolean) : if `true` then it's backward facts i.e. facts is present in object wikidata entry. For example: for the entity “Michael Faraday”, ⟨Michael Faraday, occupation, Physicist⟩ is an example of a forward fact, while ⟨Humphry Davy, student, Michael Faraday⟩ is an example of a backward fact.


### Example
```
{
  "entity_name": "Boris Pasternak",
  "qid": "Q41223",
  "sentence": "बोरिस पास्तेरनाक १९५८ में साहित्य के क्षेत्र में नोबेल पुरस्कार विजेता रहे हैं।",
  "translated_sentence": "Boris Pasternak was awarded the Nobel Prize in Literature in 1958.",
  "native_sentence_section": "introduction",
  "translated_sentence_section": "introduction",
  "sent_index": 0,
  "facts": [
    [
      "nominated for",
      "Nobel Prize in Literature",
      [
        [
          "point in time",
          "1958"
        ]
      ],
      false
    ]
  ],
  "lang": "hi",
  "sent_score": "0.82",
  "coverage": "complete"
}
```
