fuzzy-expert-system
===================

*Expert system that employs fuzzy logic using FCL standard*

Installation Guide
------------------

### Requirements

To install you need:

* Python 2.7
* WX GUI library 
* numpy library 
* PIL library 
* pylab library 
* fuzzy.storage.fcl library 
* Git
* PyScripter editor can support you by project configuration

### Preconditions

The files matchbox.fcl and matchbox-def.fcl should be defined (rules) according to the FCL standard.
The *.fcl file defines fuzzy rules e.g.:

FUNCTION_BLOCK fb

VAR_INPUT
	Endangerment: REAL; (* RANGE(0 .. 100) *)
	Similarity_Measure: REAL; (* RANGE(0 .. 100) *)
	Is_A_Sequence: REAL; (* RANGE(0 .. 15) *)
	Is_A_Cover: REAL; (* RANGE(0 .. 1) *)
	Creation_Date: REAL; (* RANGE(0 .. 365) *)
	Name_Similarity: REAL; (* RANGE(0 .. 1) *)
	Version_Priority: REAL; (* RANGE(0 .. 1) *)
	File_Extension: REAL; (* RANGE(0 .. 1) *)
	File_Size: REAL; (* RANGE(0 .. 1) *)
END_VAR

VAR_OUTPUT
	Duplicate_Image: REAL; (* RANGE(0 .. 1) *)
END_VAR


FUZZIFY	Endangerment
	TERM	Low	:=	(0, 0) (0, 1) (25, 1) (35, 0)  ;
	TERM	Middle	:=	(25, 0) (35, 1) (55, 1) (65, 0)  ;
	TERM	High	:=	(55, 0) (65, 1) (100, 1) (100, 0)  ;
END_FUZZIFY

FUZZIFY	Similarity_Measure
	TERM	No	:=	(0, 0) (1, 1) (15, 1) (20, 0)  ;
	TERM	Low	:=	(14, 0) (25, 1) (55, 1) (88, 0)  ;
	TERM	High	:=	(67, 0) (88, 1) (100, 1) (100, 0)  ;
END_FUZZIFY

FUZZIFY	Is_A_Sequence
	TERM	No_Sequence	:=	(0, 0) (1, 1) (4, 0)  ;
	TERM	Sequence_Candidate	:=	(2, 0) (4, 1) (6, 0)  ;
	TERM	Sequence	:=	(4, 0) (7, 1) (15, 0)  ;
END_FUZZIFY

FUZZIFY	Is_A_Cover
	TERM	No_Cover	:=	(4, 0) (6, 1) (10, 1) ;
	TERM	Cover	:=	(0, 1) (4, 1) (6, 0) ;
END_FUZZIFY

FUZZIFY	Creation_Date
	TERM	Similar_Date	:=	(0, 1) (92, 1) (365, 0)  ;
	TERM	Different_Date	:=	(50, 0) (100, 1) (365, 1)  ;
END_FUZZIFY

FUZZIFY	Name_Similarity
	TERM	Different_Name	:=	(0, 0) (100, 1)  ;
	TERM	Similar_Name	:=	(0, 1) (100, 0)  ;
END_FUZZIFY

FUZZIFY	Version_Priority
	TERM	Old_Version	:=	0 ;
	TERM	New_Version	:=	1 ;
END_FUZZIFY

FUZZIFY	File_Extension
	TERM	Not_Consistant	:=	0 ;
	TERM	Consistant	:=	1 ;
END_FUZZIFY

FUZZIFY	File_Size
	TERM	Not_Consistant	:=	(0, 0) (100, 1)  ;
	TERM	Consistant	:=	(0, 1) (100, 0)  ;
END_FUZZIFY

DEFUZZIFY	Duplicate_Image
	TERM	Image_Is_Not_Duplicate	:=	0 ;
	TERM	Image_Is_Duplicate	:=	1 ;
	ACCU : MAX;
	METHOD : COGS;
	DEFAULT := 0;
END_DEFUZZIFY

RULEBLOCK first

	AND : MIN;
	RULE 0: IF Similarity_Measure IS No AND Is_A_Sequence IS No_Sequence AND Is_A_Cover IS No_Cover AND Creation_Date IS Similar_Date AND Name_Similarity IS Different_Name AND Version_Priority IS Old_Version AND File_Extension IS Not_Consistant AND File_Size IS Not_Consistant THEN Duplicate_Image IS Image_Is_Not_Duplicate;
	RULE 1: IF Similarity_Measure IS No AND Is_A_Sequence IS No_Sequence AND Is_A_Cover IS No_Cover AND Creation_Date IS Similar_Date AND Name_Similarity IS Different_Name AND Version_Priority IS Old_Version AND File_Extension IS Not_Consistant AND File_Size IS Consistant THEN Duplicate_Image IS Image_Is_Not_Duplicate;
	RULE 2: IF Similarity_Measure IS No AND Is_A_Sequence IS No_Sequence AND Is_A_Cover IS No_Cover AND Creation_Date IS Similar_Date AND Name_Similarity IS Different_Name AND Version_Priority IS Old_Version AND File_Extension IS Consistant AND File_Size IS Not_Consistant THEN Duplicate_Image IS Image_Is_Not_Duplicate;
	RULE 3: IF Similarity_Measure IS No AND Is_A_Sequence IS No_Sequence AND Is_A_Cover IS No_Cover AND Creation_Date IS Similar_Date AND Name_Similarity IS Different_Name AND Version_Priority IS Old_Version AND File_Extension IS Consistant AND File_Size IS Consistant THEN Duplicate_Image IS Image_Is_Not_Duplicate;
	RULE 4: IF Similarity_Measure IS No AND Is_A_Sequence IS No_Sequence AND Is_A_Cover IS No_Cover AND Creation_Date IS Similar_Date AND Name_Similarity IS Different_Name AND Version_Priority IS New_Version AND File_Extension IS Not_Consistant AND File_Size IS Not_Consistant THEN Duplicate_Image IS Image_Is_Not_Duplicate;
	RULE 5: IF Similarity_Measure IS No AND Is_A_Sequence IS No_Sequence AND Is_A_Cover IS No_Cover AND Creation_Date IS Similar_Date AND Name_Similarity IS Different_Name AND Version_Priority IS New_Version AND File_Extension IS Not_Consistant AND File_Size IS Consistant THEN Duplicate_Image IS Image_Is_Not_Duplicate;
	RULE 6: IF Similarity_Measure IS No AND Is_A_Sequence IS No_Sequence AND Is_A_Cover IS No_Cover AND Creation_Date IS Similar_Date AND Name_Similarity IS Different_Name AND Version_Priority IS New_Version AND File_Extension IS Consistant AND File_Size IS Not_Consistant THEN Duplicate_Image IS Image_Is_Not_Duplicate;
	RULE 7: IF Similarity_Measure IS No AND Is_A_Sequence IS No_Sequence AND Is_A_Cover IS No_Cover AND Creation_Date IS Similar_Date AND Name_Similarity IS Different_Name AND Version_Priority IS New_Version AND File_Extension IS Consistant AND File_Size IS Consistant THEN Duplicate_Image IS Image_Is_Not_Duplicate;
	RULE 8: IF Similarity_Measure IS No AND Is_A_Sequence IS No_Sequence AND Is_A_Cover IS No_Cover AND Creation_Date IS Similar_Date AND Name_Similarity IS Similar_Name AND Version_Priority IS Old_Version AND File_Extension IS Not_Consistant AND File_Size IS Not_Consistant THEN Duplicate_Image IS Image_Is_Not_Duplicate;
    ...
	RULE 573: IF Similarity_Measure IS High AND Is_A_Sequence IS Sequence AND Is_A_Cover IS Cover AND Creation_Date IS Different_Date AND Name_Similarity IS Similar_Name AND Version_Priority IS New_Version AND File_Extension IS Not_Consistant AND File_Size IS Consistant THEN Duplicate_Image IS Image_Is_Duplicate;
	RULE 574: IF Similarity_Measure IS High AND Is_A_Sequence IS Sequence AND Is_A_Cover IS Cover AND Creation_Date IS Different_Date AND Name_Similarity IS Similar_Name AND Version_Priority IS New_Version AND File_Extension IS Consistant AND File_Size IS Not_Consistant THEN Duplicate_Image IS Image_Is_Duplicate;
	RULE 575: IF Similarity_Measure IS High AND Is_A_Sequence IS Sequence AND Is_A_Cover IS Cover AND Creation_Date IS Different_Date AND Name_Similarity IS Similar_Name AND Version_Priority IS New_Version AND File_Extension IS Consistant AND File_Size IS Consistant THEN Duplicate_Image IS Image_Is_Duplicate;
END_RULEBLOCK
END_FUNCTION_BLOCK

whereas rules can be automaticly generated by the Expert System.

An associated *-def.fcl file e.g.:

FUNCTION_BLOCK fb

VAR_POSITIVE_DECISION 1
	Similarity_Measure := High
	Is_A_Sequence := Sequence
	Is_A_Cover := No_Cover
	Creation_Date := Similar_Date
	Name_Similarity := Similar_Name
	Version_Priority := New_Version
	File_Extension := Consistant
	File_Size := Consistant
END_DEPENDENCY_VAR

EXPERIMENTBLOCK

Similarity_Measure;Is_A_Sequence;Is_A_Cover;Creation_Date;Name_Similarity;Version_Priority;File_Extension;File_Size;
83;8;0;2;0;1;1;10;

END_EXPERIMENTBLOCK

END_FUNCTION_BLOCK

comprises experimental input values in the 'Experimentblock'.

