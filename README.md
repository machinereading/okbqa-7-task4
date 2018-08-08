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

1. Entity Linking and Discovery (ELD)
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

2. Co-reference resolution (CR)

ELD의 필드와 동일한 정보는 생략, 추가된 정보만 기재

* entities: 개체 태깅 정보
    - ancestor: 해당 개체의 상호 참조 정보, pIdx-eIdx 형태로 표현
* pronouns: 문단 내 대명사 정보
    - proIdx: 대명사 ID
    - keyword: 대명사 명
    - ancestor: 해당 대명사의 상호 참조 정보, pIdx-eIdx 형태로 표현

##### Example
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
    
3. Zero anaphora (ZA)

ELD의 필드와 동일한 정보는 생략, 추가된 정보만 기재

* result: 무형대용어 정보
    - verbs: 문장 단위의 Predicate 정보
		+ word_id: 어절 단위 Idx
		+ ancestor: 생략된 주격 정보, pIdx-eIdx 형태로 표현
		+ verb: 동사의 원형

```
{
	"result": [{
		"verbs": [{
			"word_id": 11,
			"ancestor": ["2-0"],
			"verb": "체결",
			"cwStateCd": "FIXED",
			"cwSelIdx": 0
		}, {
			"word_id": 18,
			"ancestor": ["2-0"],
			"verb": "발매",
			"cwStateCd": "FIXED",
			"cwSelIdx": 0
		}, {
			"word_id": 21,
			"ancestor": ["2-0"],
			"verb": "데뷔",
			"cwStateCd": "FIXED",
			"cwSelIdx": 0
		}],
		"text": "[그란데]의 음악 경력으로는 《[빅토리어스]》(2011)의 사운드 트랙의 참여를 계기로 음반사 [리퍼블릭_레코드]와 계약을 체결했고, [2013년_3월]에는 그녀의 첫 정규 앨범 《[Yours_Truly]》를 발매하며 [빌보드_200] 1위로 데뷔했다."
	}, {
		"verbs": [{
			"word_id": 1,
			"ancestor": "-2",
			"verb": "폭넓",
			"cwStateCd": "FIXED",
			"cwSelIdx": []
		}, {
			"word_id": 4,
			"ancestor": "-2",
			"verb": "연상",
			"cwStateCd": "FIXED",
			"cwSelIdx": 0
		}, {
			"word_id": 7,
			"ancestor": ["2-0"],
			"verb": "받",
			"cwStateCd": "FIXED",
			"cwSelIdx": 0
		}],
		"text": "그녀의 폭넓은 음역대는 [머라이어_캐리]를 연상시키며 비평가들의 호평을 받았고 <리드_싱글> 〈[The_Way]〉는 [빌보드_핫_100] 10위권과 더블 플래티넘 인증을 받으며 성공을 거뒀다."
	}],
	"globalSId": [2663009, 2663010],
	"entities": [{
		"st": 0,
		"eIdx": 0,
		"ancestor": "-2",
		"en": 4,
		"cwStateCd": "FIXED",
		"ne_type": "PERSON",
		"cwSelIdx": [],
		"type": "ENTITY",
		"keyword": "[그란데]",
		"e_type": "NEW"
	}, {
		"st": 17,
		"eIdx": 1,
		"ancestor": "",
		"en": 24,
		"ne_type": "",
		"keyword": "[빅토리어스]",
		"e_type": "WIKILINK"
	}, {
		"st": 53,
		"eIdx": 2,
		"ancestor": "",
		"en": 63,
		"ne_type": "",
		"keyword": "[리퍼블릭_레코드]",
		"e_type": "WIKILINK"
	}, {
		"st": 75,
		"eIdx": 3,
		"ancestor": "-2",
		"en": 84,
		"cwStateCd": "FIXED",
		"ne_type": "TIME",
		"cwSelIdx": [],
		"type": "ENTITY",
		"keyword": "[2013년_3월]",
		"e_type": "NEW"
	}, {
		"st": 101,
		"eIdx": 4,
		"ancestor": "",
		"en": 114,
		"ne_type": "",
		"keyword": "[Yours_Truly]",
		"e_type": "WIKILINK"
	}, {
		"st": 122,
		"eIdx": 5,
		"ancestor": "",
		"en": 131,
		"ne_type": "",
		"keyword": "[빌보드_200]",
		"e_type": "WIKILINK"
	}, {
		"st": 155,
		"eIdx": 6,
		"ancestor": "",
		"en": 164,
		"ne_type": "",
		"keyword": "[머라이어_캐리]",
		"e_type": "WIKILINK"
	}, {
		"st": 186,
		"eIdx": 7,
		"ancestor": "",
		"en": 193,
		"ne_type": "",
		"keyword": "<리드_싱글>",
		"e_type": "ELU"
	}, {
		"st": 195,
		"eIdx": 8,
		"ancestor": "",
		"en": 204,
		"ne_type": "",
		"keyword": "[The_Way]",
		"e_type": "WIKILINK"
	}, {
		"st": 207,
		"eIdx": 9,
		"ancestor": "",
		"en": 218,
		"ne_type": "",
		"keyword": "[빌보드_핫_100]",
		"e_type": "WIKILINK"
	}],
	"wikiPageId": 725877,
	"pIdx": 2,
	"text": "[그란데]의 음악 경력으로는 《[빅토리어스]》(2011)의 사운드 트랙의 참여를 계기로 음반사 [리퍼블릭_레코드]와 계약을 체결했고, [2013년_3월]에는 그녀의 첫 정규 앨범 《[Yours_Truly]》를 발매하며 [빌보드_200] 1위로 데뷔했다. 그녀의 폭넓은 음역대는 [머라이어_캐리]를 연상시키며 비평가들의 호평을 받았고 <리드_싱글> 〈[The_Way]〉는 [빌보드_핫_100] 10위권과 더블 플래티넘 인증을 받으며 성공을 거뒀다."
}

```

4. Relation Extraction (RE)

ELD의 필드와 동일한 정보는 생략, 추가된 정보만 기재

* paragraphs: 문단 단위의 JSONObject
    - questions: 자연언어 질문에 대한 정보
		+ qid: 자연언어 질문 Idx
		+ nlq: 자연언어 질문
		+ ansArr: 여러명의 작업자가 제출한 결과들
		+ ans: Majority voting으로 얻은 최종 정답
		+ e1: Subject of triple
		+ e2: Object of triple
		+ rel: Relation of triple
		+ e1St: e1의 시작 위치 (character idx)
		+ e1En: e1의 종료 위치 (character idx)
		+ e2St: e2의 시작 위치 (character idx)
		+ e2En: e2의 종료 위치 (character idx)

```
{
	"paragraphs": [{
		"globalSId": [3208728, 3208729],
		"wikiPageId": 1020430,
		"questions": [{
			"e2St": 54,
			"nlq": "[단발령역] (이)라는 역을 운행하는 회사 또는 노선은(는) [금강산선]인가요?",
			"ansArr": ["T", "T", "T"],
			"e1St": 0,
			"ans": "T",
			"e2En": 60,
			"rel": "servingRailwayLine",
			"e1En": 5,
			"e1": "[단발령역]",
			"qid": 0,
			"e2": "[금강산선]"
		}],
		"pIdx": 1,
		"text": "[단발령역](斷髮嶺驛)은 지금의 [조선민주주의인민공화국] [강원도_(북)] [금강군]에 위치했던 [금강산선]의 [철도역]이다. 이 역과 [말휘리역] 사이는 [스위치백]으로 건설된 구간이었다."
	}, {
		"globalSId": [3717582],
		"wikiPageId": 1434327,
		"questions": [{
			"e2St": 50,
			"nlq": "[토비_레너드_무어] (이)라는 인물의 직업은(는) [배우]인가요?",
			"ansArr": ["T", "T", "T"],
			"e1St": 0,
			"ans": "T",
			"e2En": 54,
			"rel": "occupation",
			"e1En": 10,
			"e1": "[토비_레너드_무어]",
			"qid": 0,
			"e2": "[배우]"
		}],
		"pIdx": 1,
		"text": "[토비_레너드_무어](, [1981년] [4월_21일] ~ )는 [오스트레일리아] 출신의 [배우]로, [넷플릭스]의 드라마 《[데어데블_(드라마)]》에서 악당 [킹핀]의 비서 [제임스_웨슬리] 역으로 출연하였다."
	}, {
		"globalSId": [2687870],
		"wikiPageId": 737302,
		"questions": [{
			"e2St": 9,
			"nlq": "<피로트>의 국가은(는) [세르비아]인가요?",
			"ansArr": ["T", "T", "T"],
			"e1St": 0,
			"ans": "T",
			"e2En": 15,
			"rel": "country",
			"e1En": 5,
			"e1": "<피로트>",
			"qid": 0,
			"e2": "[세르비아]"
		}, {
			"e2St": 30,
			"nlq": "<피로트>를 포함하는 것은(는) [피로트_구]인가요?",
			"ansArr": ["T", "T", "T"],
			"e1St": 0,
			"ans": "T",
			"e2En": 37,
			"rel": "isPartOf",
			"e1En": 5,
			"e1": "<피로트>",
			"qid": 1,
			"e2": "[피로트_구]"
		}],
		"pIdx": 1,
		"text": "<피로트>()는 [세르비아] 남동부에 위치한 도시로, [피로트_구]의 행정 중심지이며 면적은 1,232㎢, 인구는 63,791명([2002년] 기준)이다."
	}, {
		"globalSId": [973325, 973326, 973327],
		"wikiPageId": 137382,
		"questions": [{
			"e2St": 65,
			"nlq": "<티라노사우루스>의 생물종은(는) [수각아목]인가요?",
			"ansArr": ["T", "T", "T"],
			"e1St": 0,
			"ans": "T",
			"e2En": 71,
			"rel": "order",
			"e1En": 9,
			"e1": "<티라노사우루스>",
			"qid": 0,
			"e2": "[수각아목]"
		}, {
			"e2St": 72,
			"nlq": "<티라노사우루스>의 생물종은(는) [티라노사우루스과]인가요?",
			"ansArr": ["T", "T", "T"],
			"e1St": 0,
			"ans": "T",
			"e2En": 82,
			"rel": "family",
			"e1En": 9,
			"e1": "<티라노사우루스>",
			"qid": 1,
			"e2": "[티라노사우루스과]"
		}],
		"pIdx": 1,
		"text": "<티라노사우루스>() 또는 <티라노사우루스>는 [백악기] 후기([6800~6600만_년] 전)에 살았던, [용반목] [수각아목] [티라노사우루스과]의 [속_(생물학)]이다. 종명인 <티라노사우루스>()의 일반적인 약자인 <티렉스_(밴드)>(T.rex)가 대중문화에 정착되었으며, [한국]에서는 ~사우루스를 빼고 <티라노사우루스>라고 줄여 부르기도 한다. [북아메리카_대륙]의 서쪽에서 주로 서식했으며, 다른 [티라노사우루스과]의 공룡에 비해 그 서식 범위가 넓었다(수십 평방 km에 달하는 넓은 영토를 가졌을 것으로 추측된다)."
	}, {
		"globalSId": [842625, 842626],
		"wikiPageId": 114399,
		"questions": [{
			"e2St": 198,
			"nlq": "[말랑]의 국가은(는) [인도네시아]인가요?",
			"ansArr": ["T", "T", "F"],
			"e1St": 128,
			"ans": "T",
			"e2En": 204,
			"rel": "country",
			"e1En": 132,
			"e1": "[말랑]",
			"qid": 0,
			"e2": "[인도네시아]"
		}],
		"pIdx": 8,
		"text": "<머독_대학교>은 [ACICS](Australian Consortium for 'In-Country' Indonesian Studies)라는 프로그램이 시작된 곳이기도 하다. 이 비영리 콘소시엄은 [인도네시아]의 [족자카르타]와 [말랑]에 소재한 파트너 대학들과 함께 [1994년] 설립되었으며, [머독_대학교] 학생들에게 한 학기 혹은 한 학기 이상의 [인도네시아] 지역 스터디 기회를 제공해 주고 있다."
	}, {
		"globalSId": [2037610, 2037611],
		"wikiPageId": 438064,
		"questions": [{
			"e2St": 74,
			"nlq": "[덕흥대원군]의 자녀은(는) [조선_선조]인가요?",
			"ansArr": ["F", "T", "F"],
			"e1St": 60,
			"ans": "F",
			"e2En": 81,
			"rel": "child",
			"e1En": 67,
			"e1": "[덕흥대원군]",
			"qid": 0,
			"e2": "[조선_선조]"
		}, {
			"e2St": 86,
			"nlq": "[덕흥대원군]의 자녀은(는) [하원군]인가요?",
			"ansArr": ["T", "T", "F"],
			"e1St": 60,
			"ans": "T",
			"e2En": 91,
			"rel": "child",
			"e1En": 67,
			"e1": "[덕흥대원군]",
			"qid": 1,
			"e2": "[하원군]"
		}],
		"pIdx": 1,
		"text": "[진천군] <진천군_(왕족)>(晋川君 李喜慶, [1591년] ~ [1664년])은 [조선] 중기의 왕족으로 [덕흥대원군] 증손이며, [조선_선조]의 백형 [하원군]의 손자이다. 본관은 [전주], 휘는 [희경](喜慶)이다."
	}, {
		"globalSId": [721329, 721330, 721331],
		"wikiPageId": 93892,
		"questions": [{
			"e2St": 139,
			"nlq": "[상상_속_친구들의_모험]의 방송채널은(는) [카툰_네트워크]인가요?",
			"ansArr": ["T", "T", "T"],
			"e1St": 1,
			"ans": "T",
			"e2En": 148,
			"rel": "channel",
			"e1En": 13,
			"e1": "[상상_속_친구들의_모험]",
			"qid": 0,
			"e2": "[카툰_네트워크]"
		}, {
			"e2St": 46,
			"nlq": "[카툰_네트워크_스튜디오]의 국가은(는) [미국]인가요?",
			"ansArr": ["T", "T", "T"],
			"e1St": 20,
			"ans": "T",
			"e2En": 49,
			"rel": "country",
			"e1En": 34,
			"e1": "[카툰_네트워크_스튜디오]",
			"qid": 1,
			"e2": "[미국]"
		}, {
			"e2St": 57,
			"nlq": "[카툰_네트워크_스튜디오] (이)라는 기관에서 만든 제품은(는) [애니메이션]인가요?",
			"ansArr": ["T", "T", "T"],
			"e1St": 20,
			"ans": "T",
			"e2En": 64,
			"rel": "product",
			"e1En": 34,
			"e1": "[카툰_네트워크_스튜디오]",
			"qid": 2,
			"e2": "[애니메이션]"
		}, {
			"e2St": 139,
			"nlq": "[파워퍼프걸]의 방송채널은(는) [카툰_네트워크]인가요?",
			"ansArr": ["T", "F", "T"],
			"e1St": 69,
			"ans": "T",
			"e2En": 148,
			"rel": "channel",
			"e1En": 76,
			"e1": "[파워퍼프걸]",
			"qid": 3,
			"e2": "[카툰_네트워크]"
		}],
		"pIdx": 1,
		"text": "《[상상_속_친구들의_모험]》()은 [카툰_네트워크_스튜디오]에서 기획 및 제작된 [미국]의 텔레비전 [애니메이션]이다. 《[파워퍼프걸]》을 제작하기도 한 [애니메이터] [크레이그_맥크라켄]이 제작하였다. [2004년] [8월_13일] [미국]에서 [카툰_네트워크]를 통해 첫 방영되었으며, 다른 국가들에서도 [카툰_네트워크]를 통해 방영되고 있다."
	}]
}
```

## Evaluation results (baseline)
to be announced
