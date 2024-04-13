# HORIZONTAL_miniterm_fragments
Python and PL SQL class that generates HORIZONTAL miniterm fragments from a list of predicates Pr.

class HorizontalMinitermFragmentGenerator:
    def __init__(self, predicates):
        self.predicates = predicates

    def generate_fragments(self):
        fragments = []
        for predicate in self.predicates:
            fragment = ""
            for key, value in predicate.items():
                fragment += f"{key}={value} AND "
            fragment = fragment[:-5]  
            fragments.append(fragment)
        return fragments

predicates = [
    {"Age": ">= 18", "Gender": "Male"},
    {"Age": "< 18", "Gender": "Female"},
    {"City": "New York"}
]

fragment_generator = HorizontalMinitermFragmentGenerator(predicates)
fragments = fragment_generator.generate_fragments()
print(fragments)

#PL/SQL implementation
-- PL/SQL implementation

CREATE OR REPLACE TYPE PredicateType AS OBJECT (
    key VARCHAR2(100),
    value VARCHAR2(100)
);

CREATE OR REPLACE TYPE PredicateListType AS TABLE OF PredicateType;

CREATE OR REPLACE TYPE FragmentListType AS TABLE OF VARCHAR2(4000);

CREATE OR REPLACE TYPE FragmentGenerator AS OBJECT (
    predicates PredicateListType,
    
    CONSTRUCTOR FUNCTION FragmentGenerator(p PredicatelistType) RETURN SELF AS RESULT,
    MEMBER FUNCTION GenerateFragments RETURN FragmentListType
);

CREATE OR REPLACE TYPE BODY FragmentGenerator AS
    CONSTRUCTOR FUNCTION FragmentGenerator(p PredicateListType) RETURN SELF AS RESULT IS
    BEGIN
        SELF.predicates := p;
        RETURN;
    END;

    MEMBER FUNCTION GenerateFragments RETURN FragmentListType IS
        fragments FragmentListType := FragmentListType();
        fragment VARCHAR2(4000);
    BEGIN
        FOR i IN 1..SELF.predicates.COUNT LOOP
            fragment := '';
            FOR j IN 1..SELF.predicates(i).COUNT LOOP
                fragment := fragment || SELF.predicates(i)(j).key || '=' || SELF.predicates(i)(j).value || ' AND ';
            END LOOP;
            fragment := SUBSTR(fragment, 1, LENGTH(fragment) - 5);
            fragments.EXTEND;
            fragments(fragments.LAST) := fragment;
        END LOOP;
        RETURN fragments;
    END;
END;
/
