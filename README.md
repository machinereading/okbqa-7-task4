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

2. Co-reference resolution: 이 데이터셋은 Pronoun detection, Co-reference resolution 총 2가지 Task를 수행할 수 있는 데이터셋입니다. 문단 내 등장한 대명사, 개체들의 Grouping을 할 수 있는 정보를 포함하고 있습니다.
- 자동 검출 : 대명사 후보
- Human Annotation : 자동 검출된 대명사 후보와 ELD에서 추가 태깅된 개체들의 동일 개체 선행사를 발견

3. Zero-anaphora detection: 이 데이터셋은 Intra-sentential problem, Inter-sentential problem 둘 모두를 풀 수 있는 데이터셋입니다. Case role 중 'nominative' 주격만 대상으로 합니다. 문단 내 등장한 주격 생략된 서술어 정보와 'antecedent' 선행사 정보를 포함하고 있습니다.
- 자동 검출: 주격이 생략된 서술어
- Human Annotation: 자동 검출된 서술어에 대해 생략된 주격을 발견

4. Relation Extraction: 이 데이터셋은 Distant Supervision(DS) 데이터에서 Noise filtering 문제를 풀 수 있는 데이터셋입니다. DS labeling 된 결과는 일종의 트리플로, relation의 정의문을 이용해 트리플을 자연언어 질문 형태로 변환한 다음, 작업자는 주어진 문단과 자연언어 질문을 읽고 해당 문단으로부터 알 수 있는 사실이며 그것이 '참'인지 '거짓'인지를 태깅한 정보를 포함하고 있습니다.
- 자동 검출: Distant Supervision
- Human Annotation: 주어진 자연언어 질문에 T/F 응답

## Format
TBD

## Evaluation results (baseline)
to be announced
