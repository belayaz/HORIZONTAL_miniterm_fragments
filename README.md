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
