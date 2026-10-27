# Introduction
Description of the project

# Pseudocode
```
FUNCTION read_fasta(filename):
    INITIALIZE empty sequences dictionary
    SET current_id to None

    OPEN file
    FOR each line:
        Remove surrounding whitespace
        IF line is empty:
            Skip line

        IF line starts with ">":
            Get ID from first word after ">"
            SET current_id to ID
            Initialize empty list for this ID
        ELSE:
            Append uppercase line to current sequence's list

    Join each sequence's list into one string
    RETURN sequences dictionary


FUNCTION smith_waterman(sequence1, sequence2, scoring parameters):
    INITIALIZE previous row with zeros
    SET best_score to 0

    FOR each position i in sequence1:
        INITIALIZE current row with zeros

        FOR each position j in sequence2:
            IF characters match:
                Calculate diagonal score using match score
            ELSE:
                Calculate diagonal score using mismatch penalty

            Calculate up score using gap penalty
            Calculate left score using gap penalty

            SET current cell to maximum of:
                0, diagonal score, up score, left score

            Update best_score if current cell is greater

        SET previous row to current row

    RETURN best_score


FUNCTION smith_waterman_scoring(sequence1, sequence2, scoring parameters):
    Calculate raw score using smith_waterman

    SET self1 = match_score × length of sequence1
    SET self2 = match_score × length of sequence2

    Calculate similarity = raw_score / SQRT(self1 × self2)

    RETURN similarity


FUNCTION build_distance_matrix(sequences):
    Get sequence IDs as labels
    SET n to number of sequences
    INITIALIZE n × n distance matrix with zeros

    FOR i from 0 to n - 1:
        FOR j from i + 1 to n - 1:
            Get sequences for labels[i] and labels[j]
            Calculate normalized similarity using smith_waterman_scoring
            Calculate distance = 1 - similarity

            Store distance at [i, j] and [j, i]

    RETURN distance matrix and labels

FUNCTION neighbor_joining(distance_matrix, labels):
    WHILE number of active nodes (n) > 2:
        TODO
```
# Results


# Successes
Description of the team's learning points

# Struggles
Description of the stumbling blocks the team experienced

# Personal Reflections
## Group Leader
Group leader's reflection on the project

## Other member
Other members' reflections on the project

# Generative AI Appendix
As per the syllabus
