# QUOKKA
QUerying Ontological resources and Knowledge bases for Knowledgment Augmentation



The QUOKKA workflow is a semi-automatic workflow for QUerying Ontological resources and Knowledge bases for Knowledge Augmentation. It consists in a set of SPARQL queries to perform a query expansion with the purpose of building a semantic frame of some desired domain. The QUOKKA workflow aims at building frame structures populating knowledge graphs with Linked Open Data entities from well-known and high quality semantic web resources. This allows the creation of multi-purpose and multi-modal knowledge graphs with thousands of semantic web triggers for any desired domain.



## Frame Building Workflow


![QUOKKA frame building general workflow](https://github.com/StenDoipanni/QUOKKA/quokka_general.png)


Following the above figure, starting from the upper left corner, we try to list here all the semantic relations that can be investigated about the possible relatedness of some entity and some domain.


### Manual Lexical Units Selection 
The first, exploratory, step consists in a manual selection of a set of few lexical units that are unequivocally related to the conceptual frame to be built. The best practice, coherent with the XD methodology, is to take advantage of domain experts to identify terms pointing exactly at the portion of reality that is intended to be covered by the desired to-be-built frame. This set of lexical units is, from here on, called Starting Lexical Material (SLM) set. <br>
The SLM set is furthermore expanded using the WordNet resource, which can be accessed via its online user interface8, in order to exploit relations of term hyponymity and synonymity. The rationale is that if a certain domain frame is evoked by a lexical unit, it is logically evoked by a more specified term too (this is, of course, depending on the frame and domain to be modeled). The image schematic domain offers dozens of examples: e.g. if we accept that the schema used to conceptualise “movement”, considered from a frame semantics perspective, is evoked by “walking” we have therefore to accept that it is also evoked by e.g. “running” as well as “pacing”.


### ConceptNet-driven triggering 

Each of the entries in the SLM set is then used as input variable for the next step, which consists in exploiting ConceptNet relations. The version currently available in Framester is ConceptNet 5.
The semantic relations available are:

- ```cn:DerivedFrom```: some concept is derived from some other;
- ```cn:Causes```: some entity is the cause of some concept;
- ```cn:Antonym```: some entity points at some polarity opposite to the one describe by some concept;
- ```cn:isA```: some entity is subsumed by some concept;
- ```cn:EtymologicallyRelatedTo```: the lexical unit pointing at some entity is etimologically related to the one pointing at some concept;
- ```cn:SymbolOf```: some entity is the symbol of some concept. In Peirce terms it would be more correct to say that some entity is the icon for some concept, since this relation is used mainly to connect concepts and emojis representing them;
- ```cn:UsedFor```: some entity is functional or instrumental to some concept;
- ```cn:HasSubevent```: some entity, considered in its temporal extension, therefore as event in a broad sense, is a sub-event of some concept;
- ```cn:MotivatedByGoal```: some entity is motivated by a concept, therefore conceptualized as the final goal, for the final achievement of which some form of intermediate step is therefore necessary;
- ```cn:EtymologicallyDerivedFrom```: some lexical pointer is etimologically derived from some other lexical material which points at some concept;
- ```cn:FormOf```: some entity is a form of some concept, being a variation of the original source, considering both in a comparison process which identifies some difference both on a physical or non-physical dimension.

These properties allow to exploit ConceptNet network of relations aligned to other well known semantic web and multi-modal resources, extending the SLM to a much broader triggering set.



### WikiData lexical triggering

Directly depending on the gathering of concept entities from the ConcepNet-driven triggering step, the SLM is used as input variable to retrieve all WikiData entries. Note that this is the first step that broadens the trigger set from English only to a multilingual resource.



### DBpedia factual triggering

Parallel to the WikiData lexical triggering step, it is possible, starting from ConceptNet concepts, to retrieve entities aligned to the DBpedia resource, providing a factual grounding to the domain knowledge base. This query is shown in the above figure as “DBpedia External URL query”.


### Frame-driven triggering 


On a separate branch, as shown in Fig. ??, after the SLM manual selection, in order not to re-invent the wheel, the lexical units are checked to be lexical triggers of already existing FrameNet frames. In fact, among the many existing frames some of them could have a partial overlap with the desired domain, while some of them could be sub/super sets. <br>
This step is realised in the Framester resource using as variable the lexical unit, non disambiguated, and collecting all its senses and all the frames evoked by all the senses of some lexical material. <br>
This query is represented in the above figure as the “Frames activation query” starting node. Depending on the lexical material used as input variable the amount of senses and related frames could vary from none to several frames. Further contextual disambiguation is therefore necessary, in order to improve the quality of data, and to separate the appropriate semantic sense from those not related to the domain. After performing the SPARQL query, the set of frames selected as possible triggers is therefore done manually, and after the iteration of the query for all synonyms and hyponyms, as mentioned in the Manual Lexical Units selection paragraph, the first step of the QUOKKA frame building workflow search can be declared closed, allowing to move to the following step.



### Frame element-driven triggering



Assuming that the domain to be modeled is broad enough, some parts of it could be already covered by existing frames, allowing therefore to inherit the structure of semantic roles already formalised. This step relates exactly to that eventuality: frame element activation concerns the activation of some semantic roles related to the occurrence of a ```dul:Situation```, therefore a Frame occurrence, for the domain in exam. The conceptual questions to be asked are therefore related to the structure of semantic relations involved in the domain, like “what are the necessary roles?”, or “is there an Agent / Patient?”, etc. The related SPARQL query focuses on retrieving FrameNet frame elements of type “Core”, “Extra-Thematic” and “Peripheral”, namely: those element ontologically necessary to the frame occurrence; those that specify some more or less crucial feature about both the situation or the roles involved, e.g. the degree of some action, the intensity, etc., and finally those roles which are almost always participating in frame occurrence, like time and space. In this work the semantic roles structure is inherited by both FrameNet frame elements as well as their alignments with the Framester resource. <br>
The query is shown in the above figure as “Frame Element Type Query”.


### FrameNet Lexical Units triggering


This naturally follows from previous paragraphs and from adopting a frame structure: from the online user interface of the FrameNet re-
source10 it is possible to see that some lexical units are listed as “evoking” some frame, namely, to fully understand the semantics of those lexical units it is needed some commonsense knowledge about the system of semantic relations and contexts in which they mean what they mean. This SPARQL query is meant to include as triggers of the to-be-built frame the FrameNet lexical units, declared in FrameNet as trigger of those frames that have been found as having a total or partial overlap with the desired domain (the less the domains overlap, the more it will be necessary involve humans in the loop to obtain data quality). <br>
The SPARQL query is shown in Fig. ?? as “FrameNet Lexical Unit Query” and it is useful for all those resources which are aligned to FrameNet and perform frame detection via lexical unit retrieval.



### WordNet lexical triggering 


Activation from lexical material is a substantial part of the semantic detection, and it is generated automatically re-injecting in the workflow the “Frame activation query” output results, namely the frames previously manually selected. The rationale is that, if some entity evokes a FrameNet frame, which in turn has some relation with the to-be-modeled frame, than that entity should have some form of activation to the frame itself too. Therefore, this query extends the lexical coverage from mere FrameNet lexical units to other well known semantic web resources via extracting all the elements that evoke a frame. In Framester semantics all the WordNet synsets are considered frames as well, namely, the class of situations for which a certain sense of a term is applicable: this allows an alignment from WordNet synsets, clustering lexical units with a certain sense, in turn evoking a certain frame, and the evoked frame itself. The synset (the class of situations for which a certain meaning stands) is subsumed by the frame (the class of situation satisfied by the occurrence of the frame), therefore, the SPARQL query aims at extending lexical coverage for all the senses of a certain set of terms, clustered for its context of use. Note that the amount of elements retrieved may be considerable (the broader is the to-be-modeled frame, the larger the set of triggers, possibly even thousands of WordNet synsets). Being used in many works for disambiguation, alignment and entity recognition tasks, synsets are a very important part of the knowledge base to provide a proper coverage to achieve a proper operationalisation of the frame. The WordNet version aligned in Framester is the 3.0, while the one available from the WordNet repository is the 3.1. This query could be found in the above Figure as “Frame Synsets Query”.


### Close Match triggering


Similarly to WordNet synsets, other entities from other semantic web resources are aligned to frames in the Framester hub. The alignment is done at the meta-level, using the skos:closeMatch object property, namely declaring that the concept identified by a certain URI in a resource has a close match with another concept, identified by a different URI, in another resource. The two entities are therefore still distinguished entities, but they are pointing at a the same/similar portion of reality. <br>
Here we specify entities aligned with the skos:closeMatch relation from several resources, how they interlink with each others, and the specific SPARQL query to be used for each of them:

- *WordNet synsets*: sets of contextual synonyms. As explained in previous paragraph, assuming that two lexical units can be used as synonyms in the same context, it can be inferred that the considered context is (possibly a subframe of) the to-be-modeled frame, therefore declaring the whole synset as trigger of a frame has as result a high coverage increment, including all the senses of all the terms that can be used in the similar situation. Some frames schematising events or actions can have a ```skos:closeMatch``` to verbs or nouns that points at those evants / actions. The query to retrieve WordNet synsets subsumed by some frame is the one mentioned in the previous paragraph, while to retrieve those aligned via skos, the general query is mentioned at the end of this paragraph;

- *VerbNet verbs*: verbs from the VerbNet resource can be retrieved thanks to the alignment between WordNet “word senses” and VerbNet “key senses”, as well as via the close match alignment with frames. The query to retrieve VerbNet verbs only is shown in the figure above as “VerbNet triggering”;

- *PropBank frames*: frames from the PropBank resource are aligned to FrameNet frames via the skos relation, therefore, as shown in the above figure, providing the “FrameNet frames” URIs output as input for the “PropBank triggering” SPARQL query, it is possible to collect entities from this resource;

- *BabelNet entities*: a further multilingual coverage is provided thanks to the alignment of BabelNet to Framester frames. In this way it is possible to retrieve via the skos:closeMatch alignment entities from more than 500 languages;

- *Premon entries*: an extension of the lemon model by the W3C Ontology - Lexica Community Group.

Entities from all the above mentioned resources can be retrieved via dedicated queries as well as via the skos:closeMatch query, shown in the above figure as blue oval labeled “Close-Match Query".



### YAGO Ontology triggering


Lexical grounding from WordNet is reused once again to retrieve entities from YAGO (Yet Another Great Ontology) resource. The aligned in this case is done via owl:sameAs property towards WordNet synsets. The query is shown in figure above as “YAGO Ontology query”.



### Semantic role-driven triggering


Potential roles participating in a certain frame are extracted from FrameNet frame elements (as described in paragraph “Frame element-driven triggering”), but according to Framester semantics they are not the only ones that can provide structural elements participating as roles in a frame occurrence. In fact, triggering assertions from FrameNet frame elements are extended through the multiple sources of semantic roles present in Framester: VerbNet arguments, PropBank roles and WordNet tropes. Semantic roles in Framester are organized as a complex taxonomy with a small top level that helps integrating them, and getting to the activated value. <br>
Semantic roles participating in some frame are therefore retrieved with two queries, starting from top nodes of different graphs, in order to declare the activation of both general and specific roles. <br> 
Queries to retrieve these entities can be performed both starting from frames manually selected, inheriting semantic roles, or starting from SLM used as input variable to retrieve directly roles, independently from the frame of origin. There is in fact the possibility that
the to-be-modeled frame is evoked only by a certain role of an already existing frame (e.g. those frames that represent two plausible opposite outcomes of a certain situation), for this reason, a human in the loop is necessary for this step of the knowledge base population. Possible outcomes are shown in the figure above as the output of “Semantic Role query”.


### Semantic type-driven triggering


A dimension in some way oblique to all the above mentioned ones is the semantic type of an entity.
Albeit semantic types are more productive regarding physical dimensions (e.g. spatial like ```fnst:Front```, ```fnst:Back```) or sensori-motor aspects of entities (e.g. ```fnst:Source```,
```fnst:Goal```) it is an aspect that should generally be considered in the frame building workflow, while populating the knowledge graph operationalising the frame. <br>
This step, therefore, consists in retrieving all existing FrameNet semantic types, and then manually explore their differences and coverage, resulting in a selection of semantic types eventually triggering the desired frame. Then, a second query is performed, looking for entities filtered by the aforementioned iteration of non-disambiguated lexical units from synsets and their hyponyms, also extracting their semantic type, ending in a final coherence checking between the entities retrieved, their semantic type, and their semantic type evocation of the frame. The queries are shown in Fig. ?? as “Semantic Type query”. <br>
This final query in particular, which refers to Pustejovsky’s “Type theory” is particularly relevant and is exploited to infer further knowledge from graph pattern inferences related to *Type Matching*, *Type Accomodation*, and *Type Coercion*.




A necessary concluding note to the QUOKKA workflow is that, when dealing with a considerable amount of resources, which in turn includes thousands of triples, and in addition the proposition is to deal with conceptual matter, it is important to take into account the amount of noise that can derive as a cascading consequence of different level of accuracy, multiple steps of alignments, misunderstandings, different purposes etc. In this work, to avoid the introduction of too much noise it was developed a graph accuracy scoring system, which is injected in the Framester resource as a scale from 1 to 5, and introduced in the ontology in the form of the data property: ```fschema:reliabilityScore```. <br>
The advice, when using the QUOKKA workfow applied to the Framester resource is to set, for all the queries, the value for the above mentioned data property as >= 4, in order to set the filtering granularity to maximise both accuracy and recall.


















