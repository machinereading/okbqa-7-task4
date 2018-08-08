# [OKBQA-7 Task4 : Knowledge Base Population](http://7.okbqa.org/hackathon/task/task4)

## Introduction
The knowledge base is a database that stores the expertise knowledge, rules and facts accumulated from intellectual activities and experiences related to the field in which the AI agent is used for problem solving. The knowledge base is an important element that affects the performance of various application systems based on natural language processing. The typical knowledge bases such as Wikidata, Freebase, WordNet, YAGO, Cyc, and BabelNet are widely used in English.
However, building and maintaining this massive knowledge base manually is very difficult in practice. Therefore, Entity Linking and Discovery (ELD) that can detect entities (people, events, places) from texts of Wikipedia, daily news, and Relation Extraction (RE) that can extract the relationship between the entities are very important technology for automatically expanding a knowledge base (KB Population).
Therefore, in this task, our goal is to develop various models to learn and extract knowledge from the given sentence. Especially, this hackathon is trying to solve the following important problems. 

## Task Organizers

* Prof. Key-Sun Choi, [Semantic Web Research Center](http://semanticweb.kaist.ac.kr/), [KAIST](http://www.kaist.edu).
* Ph.D Eun-kyung Kim, [Semantic Web Research Center](http://semanticweb.kaist.ac.kr/), [KAIST](http://www.kaist.edu).
* Sangha Nam, [Semantic Web Research Center](http://semanticweb.kaist.ac.kr/), [KAIST](http://www.kaist.edu).

## Datasets
여기서 제공하는 데이터셋은 한국어 위키피디아 문서 중 최대 500문단을 선정하여 크라우드소싱 방법으로 태깅 받은 데이터들입니다.
본 데이터 셋은 Reading comprehension의 대표적인 Task를 위한 벤치마크 데이터를 동일한 코퍼스 상에서 생성하고 이를 평가하는 것이 목적입니다.
모든 데이터셋 생성 시 크라우드소싱의 효율성을 높이기 위해 자동 검출 단계와 Human Annotation 단계가 복합적으로 수행되었습니다.

1. Entity Linking and Discovery(ELD): 이 데이터셋은 Entity boundary detection, Entity linking, Entity discovery 총 3가지 Task를 수행할 수 있는 데이터셋입니다. Plain text에서 Entity mention에 대한 boundary span 정보를 기본적으로 제공하며, 각 mention마다 한국어 디비피디아 지식베이스에 연결되는 Specific entity가 무엇인지와 해당 entity의 Ontological Type 정보가 포함되어 있습니다. 또한, 디비피디아 지식베이스에 연결된 개체와 연결이 되지못한 개체의 경우에는 PLOMT(Person, Location, Organisation, Miscellaneous, Time) Type 정보를 포함하고 있습니다.
- 자동 검출 : 위키피디아 텍스트 상 Wikilink 정보, Entity Linking 결과
- Human Annotation : 자동 검출되지 않은 추가적인 개체들, 자동 검출된 개체 중 틀린 것 제거

2. Co-reference resolution(CR): 이 데이터셋은 Pronoun detection, Co-reference resolution 총 2가지 Task를 수행할 수 있는 데이터셋입니다. 문단 내 등장한 대명사, 개체들의 Grouping을 할 수 있는 정보를 포함하고 있습니다.
- 자동 검출 : 대명사 후보
- Human Annotation : 자동 검출된 대명사 후보와 ELD에서 추가 태깅된 개체들의 동일 개체 선행사를 발견

3. Zero-anaphora detection(ZA): 이 데이터셋은 Intra-sentential problem, Inter-sentential problem 둘 모두를 풀 수 있는 데이터셋입니다. Case role 중 'nominative' 주격만 대상으로 합니다. 문단 내 등장한 주격 생략된 서술어 정보와 'antecedent' 선행사 정보를 포함하고 있습니다.
- 자동 검출: 주격이 생략된 서술어
- Human Annotation: 자동 검출된 서술어에 대해 생략된 주격을 발견

4. Relation Extraction(RE): 이 데이터셋은 Distant Supervision(DS) 데이터에서 Noise filtering 문제를 풀 수 있는 데이터셋입니다. DS labeling 된 결과는 일종의 트리플로, relation의 정의문을 이용해 트리플을 자연언어 질문 형태로 변환한 다음, 작업자는 주어진 문단과 자연언어 질문을 읽고 해당 문단으로부터 알 수 있는 사실이며 그것이 '참'인지 '거짓'인지를 태깅한 정보를 포함하고 있습니다.
- 자동 검출: Distant Supervision
- Human Annotation: 주어진 자연언어 질문에 T/F 응답

## Format
모든 데이터는 JSON 형태로 제공됩니다.

1. Entity Linking
* plainText: 원본 텍스트
* mentionedTagged: Entity Mention의 Boundary가 []로 태깅된 텍스트
* entityTagged: Entity Mention을 개체명으로 변경한 []로 태깅된 텍스트
* wikiPageId: 원본 텍스트의 위키피디아 페이지 ID
* pIdx: 원본 텍스트의 위키피디아 문단 ID
* entities: 개체 태깅 정보
    - eIdx: 개체의 ID
    - e_type: 개체의 대분류, "WIKILINK", "ELU", "NEW" 3가지 속성을 가지며, 각각 위키피디아 위키링크, 자동 개체 연결, 사람 신규 태깅을 의미한다.
    - keyword: 개체명
    - ne_type: Named Entity Type (PLOMT)
    - kbox_types: 개체의 DBpedia Ontology Type
    - st_mention: plainText 상에서의 개체 시작 Idx (character)
    - en_mention: plainText 상에서의 개체 종료 Idx (character)
    - st: entityTagged 상에서의 개체 시작 Idx
    - en: entityTagged 상에서의 개체 종료 Idx
    
##### Example
```
{
	"entityTagged": "자녀. [필리포]는 [1739년] [10월_25일] [루이_15세]의 딸 [프랑스의_엘리사베타]와 결혼했다.",
	"globalSId": [2364492, 2364493],
	"entities": [{
		"kbox_types": ["SoccerPlayer", "SportsManager", "Agent", "Person", "SoccerManager", "Athlete"],
		"st": 4,
		"en_mention": 7,
		"ancestor": "",
		"eIdx": 0,
		"st_mention": 4,
		"en": 9,
		"ne_type": "PERSON",
		"keyword": "[필리포]",
		"e_type": "NEW"
	}, {
		"kbox_types": ["Year", "TimePeriod"],
		"st": 11,
		"en_mention": 14,
		"ancestor": "",
		"eIdx": 1,
		"st_mention": 9,
		"en": 18,
		"ne_type": "TIME",
		"keyword": "[1739년]",
		"e_type": "WIKILINK"
	}, {
		"kbox_types": ["NULL"],
		"st": 19,
		"en_mention": 22,
		"ancestor": "",
		"eIdx": 2,
		"st_mention": 15,
		"en": 28,
		"ne_type": "TIME",
		"keyword": "[10월_25일]",
		"e_type": "WIKILINK"
	}, {
		"kbox_types": ["Royalty", "Monarch", "BritishRoyalty", "Agent", "Person"],
		"st": 29,
		"en_mention": 29,
		"ancestor": "",
		"eIdx": 3,
		"st_mention": 23,
		"en": 37,
		"ne_type": "PERSON",
		"keyword": "[루이_15세]",
		"e_type": "WIKILINK"
	}, {
		"kbox_types": ["Royalty", "BritishRoyalty", "Agent", "Person"],
		"st": 41,
		"en_mention": 38,
		"ancestor": "",
		"eIdx": 4,
		"st_mention": 33,
		"en": 53,
		"ne_type": "PERSON",
		"keyword": "[프랑스의_엘리사베타]",
		"e_type": "WIKILINK"
	}],
	"wikiPageId": 585033,
	"pIdx": 3,
	"plainText": "자녀. 필리포는 1739년 10월 25일 루이 15세의 딸 엘리사베타와 결혼했다.",
	"mentionTagged": "자녀. [필리포]는 [1739년] [10월 25일] [루이 15세]의 딸 [엘리사베타]와 결혼했다."
}
```

2. Co-reference resolution
ELD의 필드와 동일한 정보는 생략, 추가된 정보만 기재
* entities: 개체 태깅 정보
    - ancestor: 해당 개체의 상호 참조 정보, pIdx-eIdx 형태로 표현
* pronouns: 문단 내 대명사 정보
    - proIdx: 대명사 ID
    - keyword: 대명사 명
    - ancestor: 해당 대명사의 상호 참조 정보, pIdx-eIdx 형태로 표현

```
{
	"entityTagged": "[필리포 1세](Felipe I de Parma, [1720년] [3월_15일] ~ [1765년] [7월_18일])는 <이탈리아>의 귀족으로 [부르봉파르마] 가문의 시조였다. [스페인]의 부르봉왕가 초대 국왕 [펠리페_5세]와 그의 아내 [이사벨_디_파르네시오] 사이에서 태어났으며, 후에 [파르마의_군주]이 되었다.",
	"globalSId": [2364485, 2364486],
	"entities": [{
		"kbox_types": ["NULL"],
		"st": 0,
		"en_mention": 6,
		"ancestor": "-2",
		"eIdx": 0,
		"st_mention": 0,
		"en": 8,
		"ne_type": "PERSON",
		"keyword": "[필리포 1세]",
		"e_type": "NEW"
	}, {
		"kbox_types": ["Year", "TimePeriod"],
		"st": 28,
		"en_mention": 31,
		"ancestor": "",
		"eIdx": 1,
		"st_mention": 26,
		"en": 35,
		"ne_type": "TIME",
		"keyword": "[1720년]",
		"e_type": "WIKILINK"
	}, {
		"kbox_types": ["NULL"],
		"st": 36,
		"en_mention": 38,
		"ancestor": "",
		"eIdx": 2,
		"st_mention": 32,
		"en": 44,
		"ne_type": "TIME",
		"keyword": "[3월_15일]",
		"e_type": "WIKILINK"
	}, {
		"kbox_types": ["Year", "Agent", "EducationalInstitution", "TimePeriod", "Organisation", "University"],
		"st": 47,
		"en_mention": 46,
		"ancestor": "",
		"eIdx": 3,
		"st_mention": 41,
		"en": 54,
		"ne_type": "TIME",
		"keyword": "[1765년]",
		"e_type": "WIKILINK"
	}, {
		"kbox_types": ["NULL"],
		"st": 55,
		"en_mention": 53,
		"ancestor": "",
		"eIdx": 4,
		"st_mention": 47,
		"en": 63,
		"ne_type": "TIME",
		"keyword": "[7월_18일]",
		"e_type": "WIKILINK"
	}, {
		"kbox_types": ["Country", "PopulatedPlace", "Settlement", "PopulatedPlace", "Place"],
		"st": 66,
		"en_mention": 60,
		"ancestor": "",
		"eIdx": 5,
		"st_mention": 56,
		"en": 72,
		"ne_type": "LOCATION",
		"keyword": "<이탈리아>",
		"e_type": "ELU"
	}, {
		"kbox_types": ["NULL"],
		"st": 79,
		"en_mention": 73,
		"ancestor": "-2",
		"eIdx": 6,
		"st_mention": 67,
		"en": 87,
		"ne_type": "ETC",
		"keyword": "[부르봉파르마]",
		"e_type": "NEW"
	}, {
		"kbox_types": ["Country", "PopulatedPlace", "Settlement", "PopulatedPlace", "Place"],
		"st": 98,
		"en_mention": 87,
		"ancestor": "",
		"eIdx": 7,
		"st_mention": 84,
		"en": 103,
		"ne_type": "LOCATION",
		"keyword": "[스페인]",
		"e_type": "WIKILINK"
	}, {
		"kbox_types": ["Royalty", "BritishRoyalty", "Agent", "Person"],
		"st": 117,
		"en_mention": 107,
		"ancestor": "",
		"eIdx": 8,
		"st_mention": 101,
		"en": 125,
		"ne_type": "PERSON",
		"keyword": "[펠리페_5세]",
		"e_type": "WIKILINK"
	}, {
		"kbox_types": ["Royalty", "BritishRoyalty", "Agent", "Person"],
		"st": 133,
		"en_mention": 126,
		"ancestor": "",
		"eIdx": 9,
		"st_mention": 115,
		"en": 146,
		"ne_type": "PERSON",
		"keyword": "[이사벨_디_파르네시오]",
		"e_type": "WIKILINK"
	}, {
		"kbox_types": ["NULL"],
		"st": 162,
		"en_mention": 148,
		"ancestor": "",
		"eIdx": 10,
		"st_mention": 142,
		"en": 171,
		"ne_type": "ETC",
		"keyword": "[파르마의_군주]",
		"e_type": "WIKILINK"
	}],
	"wikiPageId": 585033,
	"pIdx": 1,
	"plainText": "필리포 1세(Felipe I de Parma, 1720년 3월 15일 ~ 1765년 7월 18일)는 이탈리아의 귀족으로 부르봉파르마 가문의 시조였다. 스페인의 부르봉왕가 초대 국왕 펠리페 5세와 그의 아내 이사벨 디 파르네시오 사이에서 태어났으며, 후에 파르마 공작이 되었다.",
	"pronouns": [{
		"st": 127,
		"ancestor": "1-8",
		"proIdx": 0,
		"en": 127,
		"cwStateCd": "FIXED",
		"cwSelIdx": 18,
		"type": "PRONOUN",
		"keyword": "그"
	}, {
		"st": 127,
		"ancestor": "-2",
		"proIdx": 1,
		"en": 131,
		"cwStateCd": "FIXED",
		"cwSelIdx": [],
		"type": "PRONOUN",
		"keyword": "그의 아내"
	}, {
		"st": 127,
		"ancestor": "-2",
		"proIdx": 2,
		"en": 145,
		"cwStateCd": "FIXED",
		"cwSelIdx": [],
		"type": "PRONOUN",
		"keyword": "그의 아내 [이사벨_디_파르네시오]"
	}, {
		"st": 127,
		"ancestor": "-2",
		"proIdx": 3,
		"en": 150,
		"cwStateCd": "FIXED",
		"cwSelIdx": [],
		"type": "PRONOUN",
		"keyword": "그의 아내 [이사벨_디_파르네시오] 사이에서"
	}],
	"mentionTagged": "[필리포 1세](Felipe I de Parma, [1720년] [3월 15일] ~ [1765년] [7월 18일])는 [이탈리아]의 귀족으로 [부르봉파르마] 가문의 시조였다. [스페인]의 부르봉왕가 초대 국왕 [펠리페 5세]와 그의 아내 [이사벨 디 파르네시오] 사이에서 태어났으며, 후에 [파르마 공작]이 되었다."
}
```
    


## Evaluation results (baseline)
to be announced
