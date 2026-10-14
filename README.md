# Introduction
This project implements a De Bruijn graph genome assembler in a DeBruijnGraph class and uses it to assemble the Saccharomyces cerevisiae R64 genome from 2,414,266 simulated, error-free, 150 bp single-end reads at 30x coverage. Each read is broken into k-mers (k = 31), and each k-mer becomes a directed edge between its left and right (k−1)-mers. Contigs are assembled by recursive Eulerian walks through the graph, split wherever the walk joins nodes that are not connected, and converted back to DNA sequence. The class also calculates assembly statistics, including N50, and writes the contigs to a FASTA file.

All required functionality described in the assignment instructions was completed. All eight methods of the DeBruijnGraph class were implemented, the toy example assembles its 9 reads into a single contig, and the yeast assembly runs to completion in about 8 minutes. The Eulerian walk is recursive, as the assignment requires. The optional mouse challenge was not attempted.


# Pseudocode
*Note: this is the original pseudocode, written before coding began. It does not reflect the changes made to the code during troubleshooting: raising the recursion limit, adding both DNA strands to the graph, removing twin edges during the walk, and reusing sub-tours instead of copying them in the recursive walk. These changes and the reasons for them are described in the Struggles section.  Original was kept as a way to see the differences in orginial concept and working program.*

```
#Project04 Pseudocode

"""De Bruijn Graph Genome Assembly Module.

This module provides classes and functions for constructing De Bruijn graphs
from sequencing reads and performing genome assembly using Eulerian path
traversal.
"""
from collections import defaultdict
import random


class DeBruijnGraph:
    """Main class for De Bruijn graphs and genome assembly.

    This class builds De Bruijn graphs from sequencing reads and performs
    genome assembly by finding Eulerian paths through connected components
    of the graph.

    Attributes:
        graph (defaultdict): Adjacency list representation of graph edges.
            Keys are (k-1)-mers, values are lists of adjacent (k-1)-mers.
        k (int): The k-mer size used for graph construction.

    Example:
        >>> reads = ["ATGGCGTACG", "GCGTACGTTA", "ACGTTACCAT"]
        >>> dbg = DeBruijnGraph(reads, k=6)
        >>> contigs = dbg.assemble_contigs(seed=42)
        >>> len(contigs) > 0
        True
    """

    def __init__(self, reads, k):
        """Initialize De Bruijn graph from sequencing reads.

        Args:
            reads (list): List of DNA sequence strings.
            k (int): K-mer size for graph construction.

        Example:
            >>> reads = ["ATGGCG", "GCGTGC", "TGCAAC"]
            >>> dbg = DeBruijnGraph(reads, k=4)
            >>> len(dbg.graph) > 0
            True
        """
        self.graph = defaultdict(list)
        self.k = k
        self.build_graph_from_reads(reads, k)

    def add_edge(self, left, right):
        """Add a directed edge to the graph.

        Args:
            left (str): Source (k-1)-mer node.
            right (str): Destination (k-1)-mer node.

        Example:
            >>> dbg = DeBruijnGraph([], k=4)
            >>> dbg.add_edge("ATG", "TGG")
            >>> "TGG" in dbg.graph["ATG"]
            True
        """

        #add right to the end of left's list in self.graph
    
    def remove_edge(self, left, right):
        """Remove a directed edge from the graph.

        Args:
            left (str): Source (k-1)-mer node.
            right (str): Destination (k-1)-mer node.

        Example:
            >>> dbg = DeBruijnGraph([], k=4)
            >>> dbg.add_edge("ATG", "TGG")
            >>> dbg.remove_edge("ATG", "TGG")
            >>> len(dbg.graph["ATG"])
            0
        """
        #remove right from left's list in self.graph

    def build_graph_from_reads(self, reads, k):
        """Build De Bruijn graph from multiple sequencing reads.

        Extracts all k-mers from all reads and adds edges between
        consecutive (k-1)-mers within each k-mer.

        Args:
            reads (list): List of DNA sequence strings.
            k (int): K-mer length for graph construction.

        Example:
            >>> reads = ["ATGGC", "TGGCA"]
            >>> dbg = DeBruijnGraph([], k=4)
            >>> dbg.build_graph_from_reads(reads, 4)
            >>> "ATG" in dbg.graph
            True
        """

        #Sanity-check print statement:  print how many reads there are and the value of k
        #FOR each read in reads:
            #FOR each position, from 0 up to (length of read - k):
                #kmer = the k letters starting at this position
                #left = kmer without its last letter
                #right = kmer without its first letter
                #Sanity-check print statement: print each kmer with its left and right node, first read only
                #IF right is not already in left's list in self.graph:
                    #self.add_edge(left, right)
        #Sanity-check print statement: pring number of nodes, number of edges

    def eulerian_walk(self, node, graph, seed=None):
        """Perform recursive Eulerian walk on a graph component.

        This is a recursive function that follows all edges from a node
        to traverse the graph, building a path in reverse order.

        Args:
            node (str): Current node to traverse from.
            graph (defaultdict): Graph or subgraph to traverse.
            seed (int, optional): Seed for random edge selection.

        Returns:
            list: List of (k-1)-mers traversed (in reverse order).

        Example:
            >>> reads = ["ATGGCG"]
            >>> dbg = DeBruijnGraph(reads, k=4)
            >>> graph_copy = defaultdict(list, dbg.graph)
            >>> tour = dbg.eulerian_walk("ATG", graph_copy, seed=42)
            >>> len(tour) > 0
            True
        """
        #IF a seed was given:
            #set the random seed (only in 1st call)
        #tour = an empty list

        #WHILE node's list in graph is not empty:
            #next node = pick one of node's list in graph at random
            #remove next node from node's list in graph (the copy)
            #tour = tour + self.eulerian_walk(next node, graph)

        #add node to end of tour
        #Return tour

    def assemble_contigs(self, seed=None):
        """Assemble all contigs from the De Bruijn graph.

        Finds all connected components and generates an Eulerian path
        for each component, producing multiple assembled contigs.

        Args:
            seed (int, optional): Random seed for reproducible assembly.

        Returns:
            list: List of assembled contig sequences (DNA strings).

        Example:
            >>> reads = ["ATGGCGTACG", "GCGTACGTTA", "ACGTTACCAT"]
            >>> dbg = DeBruijnGraph(reads, k=6)
            >>> contigs = dbg.assemble_contigs(seed=42)
            >>> all(isinstance(c, str) for c in contigs)
            True
        """
        #IF a seed was given:
            #set the random seed (once for all walks)
        #copy = a new empty graph
        #FOR each node in self.graph:
            #copy's list for node = a copy of node's list
        #Sanity-check print statement: print node count in self.graph and in copy (should match)
        
        #arriving = a count for each node, starting at 0
        #FOR each node in self.graph:
            #FOR each neighbor in node's list:
                #add 1 to arriving for neighbor
        
        #start nodes = every node whose list length (leaving) > its arriving count
        #Sanity-check print statement: print how many start nodes were found

        #all nodes = a list of every node in self.graph, made now, before any walking 
        #contigs = an empty list
        #kept = an empty set

        #FOR each node in (start nodes, followed by all nodes):
            #IF node's list in copy is not empty:
                #tour = self.eulerian_walk (node, copy)
                #Sanity-check print statement: print the length of the tour (first 3 walks only)
                
                #reverse the tour
                #pieces = a list holding one piece: [the first node]
                #FOR each node in the tour after the first:
                    #previous = the last node of the current piece
                    #IF node is in previous's list in self.graph: (real edge, the original)
                        #add node to the current piece
                    #ELSE:
                        #start a new piece with node
                
                #FOR each piece:
                    #IF the piece has at least 2 nodes:
                        #sequence = self.tour_to_sequence(piece)
                        #rc = the reverse complement of sequence
                        #IF neighter sequence or rc is in kept:
                            #add sequence to contigs
                            #add sequence to kept
        
        #Sanity check print statement: print number of contigs
        #Sanity check print statement: print edge count in self.graph (should be unchanged, proving original is intact)
        #Return contigs


    def tour_to_sequence(self, tour):
        """Convert a tour of (k-1)-mers into a DNA sequence.

        Args:
            tour (list): List of (k-1)-mer strings in order.

        Returns:
            str: Assembled DNA sequence.

        Example:
            >>> dbg = DeBruijnGraph([], k=4)
            >>> tour = ['ATG', 'TGG', 'GGC', 'GCG']
            >>> dbg.tour_to_sequence(tour)
            'ATGGCG'
        """
        #sequence = the whole first node in tour
        #FOR each node after the first one:
            #add that node's last letter to the end of sequence
        #Return sequence

    def get_assembly_stats(self, contigs):
        """Calculate assembly statistics for assembled contigs.

        Args:
            contigs (list): List of contig sequences.

        Returns:
            dict: Dictionary containing assembly statistics:
                - num_contigs: Total number of contigs
                - total_length: Total assembled sequence length
                - longest_contig: Length of longest contig
                - shortest_contig: Length of shortest contig
                - mean_length: Mean contig length
                - n50: N50 statistic

        Example:
            >>> contigs = ["ATGGCG", "TTTAAA", "CCCCCCCCCC"]
            >>> dbg = DeBruijnGraph([], k=4)
            >>> stats = dbg.get_assembly_stats(contigs)
            >>> stats['num_contigs']
            3
        """
        #lengths = a list of the length of each contig

        #IF there are no contigs:
            #Return all six stats set to 0

        #num_contigs = how many contigs are there
        #total_length = all lengths added together
        #longest_contig = the biggest length
        #shortest_contig = the smallest length
        #mean_length = total_length / num_contigs

        #sort the lengths, longest first
        #running total = 0
        #FOR each length:
            #add it to the running total
            #IF running total >= total_length / 2
                #n50 = this length
                #stop the loop
        
        #Return a dictionary holding the six stats, under these exact names:
            #num_contigs, total_length, longest_contig, shortest_contig, mean_length, n50

    def write_fasta(self, contigs, filename):
        """Write assembled contigs to a FASTA file.

        Args:
            contigs (list): List of contig sequences.
            filename (str): Output FASTA filename.

        Example:
            >>> contigs = ["ATGGCG", "TTTAAA"]
            >>> dbg = DeBruijnGraph([], k=4)
            >>> dbg.write_fasta(contigs, "output.fasta")
        """
        #open the file called filename, for writing
        #FOR each contig, numbered from 1:
            #write ">contig_" + number + " length=" + length of contig, then a line break
            #write the contig, then a line break
        #close the file
        #Sanity-print check statement:  print the filename and how many contigs were written
```
# Dependencies
Python and standard libraries

# Results
================================================================================
GENOME DE BRUIJN GRAPH ASSEMBLY
Student Assignment Driver Program
================================================================================

================================================================================
GENOME ASSEMBLY PIPELINE
================================================================================
Start time: 2026-10-13 05:43:35

STEP 1: Loading sequencing reads
--------------------------------------------------------------------------------
Input file: data/sacCer_R64_150SE_30x_wgsim.fq.gz

Reads loaded: 2,414,266
Total bases: 362,139,900 bp
Average read length: 150.0 bp
Time elapsed: 3.91 seconds

Sample reads:
  First: ATCACAAAGGAGACAAAATCGTTGATAAAAAGTGCATCAACATTCCCAGCCAGAAAATGCACATCATAAAGACATGTTAT...
  Last:  ATTTAGCTCGCTTGTTTCTTGATTTTTTAGTCTCAATTTCATATCGTTAATAAAATCCTTGTGAGCTTCTGAGTTTAAAG...

STEP 2: Building De Bruijn graph
--------------------------------------------------------------------------------
K-mer size: 31
Building graph from 2,414,266 reads...

Graph construction complete!
  Nodes (distinct 30-mers): 22,953,466
  Edges (distinct 31-mers):   22,961,448
  Average out-degree: 1.00
Time elapsed: 366.01 seconds

STEP 3: Assembling contigs
--------------------------------------------------------------------------------
Traversing the graph to build contigs...

Assembly complete!
  Contigs generated: 3,844
Time elapsed: 122.53 seconds

STEP 4: Calculating assembly statistics
--------------------------------------------------------------------------------
Assembly statistics:
  Number of contigs:     3,844
  Total assembly length: 11,592,504 bp
  Longest contig:        108,052 bp
  Shortest contig:       31 bp
  Mean contig length:    3,015.7 bp
  N50:                   25,294 bp

Top 10 longest contigs:
   1. 108,052 bp
   2. 104,496 bp
   3. 93,617 bp
   4. 79,109 bp
   5. 78,359 bp
   6. 77,240 bp
   7. 75,690 bp
   8. 75,295 bp
   9. 74,255 bp
  10. 73,614 bp

Genome coverage analysis:
  Reference genome size:         12,071,326 bp
  Estimated sequencing coverage: 30.0x
  Assembly size vs. genome:      96.0%
  (Near 200% usually means reverse-complement duplicate contigs were not removed.)

STEP 5: Writing output files
--------------------------------------------------------------------------------
✓ Contigs written to: genome_assembly.fasta
✓ Statistics written to: genome_assembly_stats.txt

================================================================================
ASSEMBLY COMPLETE
================================================================================
Total time (excluding file output): 492.45 seconds (8.21 minutes)
End time: 2026-10-13 05:51:57

================================================================================
FIVE LONGEST ASSEMBLED CONTIGS
================================================================================

>contig_1 length=108052
CCTTGTTCGTTGGCAATAATTTCAACGGAGGATTCGTAAGTAGCAACACAAGAGTAGGTTGTACCTAAATCGATACCGATAGCACCTTGGAAAACACCTT...

>contig_2 length=104496
AATGGTCACGGCGTCTGGCTACGAACCAGGAGATTCCAGGTTCGAGTCCTGGCGGGGAAGAATATTCTTTTTGTCACTAACACAACCATTCTATAAATTG...

>contig_3 length=93617
ACTTGGCTCTCAAATCAAGTTATAATTGTATCGGAGAACTACCATTCACCAATTTCACACCGTCATTCACAGATGTTATCGACTATATATGGTTTTCTAC...

>contig_4 length=79109
AATAATAATAATAATAATAGTAATAATAACGTCTATATAAGAAGTATCTACACCATTGCTTACGCTTTTATTTTTTCCTTTCGACGTGAAAAGCGATAAT...

>contig_5 length=78359
TGCTGTTGCTGTTGTTGTTGTTGTTGTTGCTGATGCTGCTGCTGCTGTTGTTGTTGCTTTTGTTGCTGCTGTTGTATTTGCACATGATGCCTAAACTGTT...

================================================================================
✓ ASSEMBLY COMPLETE
================================================================================

Results saved to:
  • genome_assembly.fasta - 3,844 assembled contigs
  • genome_assembly_stats.txt - detailed assembly statistics

Key metrics:
  • Total assembly: 11,592,504 bp
  • N50: 25,294 bp
  • Longest contig: 108,052 bp
  • Coverage: 30.0x

# Evidence and Reasoning
The toy example assembled its 9 overlapping reads into one contig, ATGGCGTACGTTACCATGGGCCTAAA. Before the yeast run, the code was tested on a simulated 300,000 bp genome with reads from both strands at 30x coverage, where it assembled 100.0% of the genome in a single contig.

Claude was used to help identify areas where extra sanity-check print statements could be inserted into the code to confirm each stage of the yeast run was working as intended. The graph copy had the same number of nodes as the original (22,953,458), and the original graph still had all 22,961,448 edges after assembly, showing that the walk only modified the copy. The first walk reached 8,918,336 nodes, which confirmed that the raised recursion limit was necessary. The print statements were commented out once the code was confirmed working, and the results were identical with them on and off.

The assembly totals 11,592,504 bp, or 96.0% of the 12,071,326 bp reference. Assemblies near 200% indicate that reverse-complement duplicates were not removed, so 96.0% shows that each region was assembled once. The missing ~4% is expected for this approach. Each distinct k-mer is stored as a single edge, so sequence that appears more than once in the genome, like transposable elements and ribosomal DNA repeats, are assembled only once. Each split at a false join also loses the one edge leading into the new piece, and pieces with only one node are not reported as contigs.

Repeats can also cause misassembly. Where two regions share a repeat, the walk can follow real edges that join those regions in the wrong order, producing a contig in which each step is valid in the graph but the overall sequence does not match the genome. This is a known limitation of Eulerian-path assembly. Our testing did not include comparing the yeast contigs against the reference, so the number of misassembled contigs is unknown.

# Successes
The assembler produced a correct result on the toy example and assembled 96.0% of the yeast genome in 3,844 contigs, with an N50 of 25,294 bp and a longest contig of 108,052 bp, in about 8 minutes. The walk is fully recursive, and all eight methods share one graph stored on the object, meeting the assignment's recursion and object-oriented requirements.

Testing on a simulated genome with a known answer was the most useful step in the project. It found the reverse-complement duplication problem and the slow list concatenation in the walk before the full yeast run, neither of which the toy example could reveal. The sanity-check print statements, ideas over from past projects, made it possible to confirm the graph copy, the unchanged original graph, and the recursion depth on the real data.

# Reflection and Adaptive Learning
All four problems described in Struggles appeared only at realistic data scale. In future projects, it would be best to test on simulated data of realistic size, with a known answer, as soon as the code works on the toy example, and to estimate memory use and recursion depth from the data before a full run.

# Struggles
The four struggles and fixes below were worked out with the help of Claude.

### Recursion Limit
Problem: Python allows 1,000 nested function calls by default, and the recursive walk makes one nested call per step along a contig. The yeast graph is long, unbranched chains, and the first walk reached 8,918,336 calls deep, far past both the default and the 1,000,000 limit first tried. A classmate recommended a much higher limit.

Fix: sys.setrecursionlimit(50000000). A walk cannot be deeper than the total number of edges (~23 million), so a correct walk never reaches this limit, and it still stops a runaway recursion caused by a bug.

What was learned: the recursion limit is a safeguard against runaway recursion, not a measure of what the computer can handle. Recursion depth depends on the shape of the data, which a 22-node toy example cannot show.

### Both strands (reverse complement duplicates)
Problem: reads come from both strands, so each region can be assembled twice, once as itself and once as its reverse complement. The first approach, skipping a contig if its exact reverse complement was already kept, gave 200% on a simulated genome. Coverage gaps on one strand split it into pieces that no longer exactly matched the other strand's contig.

Fix, part 1: build_graph_from_reads also adds the reverse complement of every read, so both strands get full coverage.

Fix, part 2: whenever eulerian_walk uses an edge, it also removes the twin edge on the other strand, revcomp(right) to revcomp(left), so the second strand is never assembled separately. Part 1 alone still gave 198% on a genome with repeats. With both parts, the simulated genome assembled at 100% and the yeast genome at 96.0%.

What was learned: a check that depends on an exact match can fail without any error on real data. Testing on simulated data with a known answer caught a problem the toy example could not.

### False join splitting
Problem: repeats create branches in the graph, and a group of nodes may then have no single path through every edge. The walk can place two nodes next to each other in the tour even though no edge connects them, which would produce sequence that does not exist in the genome.

Fix: after reversing each tour, assemble_contigs checks every neighboring pair against the original graph and starts a new piece wherever the pair is not a real edge. This requires the walk to use a separate copy of the graph, made with ```list()``` so each node's list is copied, because the walk empties the copy and only the original can confirm which edges are real.

What was learned: an algorithm can be correct in theory and still give wrong output when real data breaks its assumptions.

### Performance (list copying in the recursive walk)
Problem: ```tour = tour + self.eulerian_walk(...)``` creates a new list on every recursive call, copying every node collected so far. Tested on long chains, the time grew about 5x each time the length doubled (20 seconds for 80,000 nodes), which would have taken hours on yeast walks millions of nodes deep.

Fix: reuse the list returned by the inner call (```tour = sub_tour```) and use ```tour.extend(sub_tour)``` only at branch points. The 80,000-node chain dropped to 0.1 seconds, and the yeast assembly step took about 2 minutes. The walk is still recursive and gives the same result.

What was learned: correct code can still be unusable at scale, and testing on data closer to the real size is necessary before a full run.

# Personal Reflections
## Group Leader - Matthew (Jake)
This was my first time as a project leader and the process went pretty well. At first I struggled to figure out what files needed to be uploaded to github. I didn't want to add the data files as this seemed unnecessary, and we ended up just keeping a few files there (mainly README.md and project04.ipynb) and storing the data files locally. This seemed to work and is something I will continue to do moving forward. 

In terms of implementation of the algorithm, due to conflicting schedules the group worked on the project in parallel. I struggled a lot with the basic structure of the algorithm. In praticular, I didnt understand that each read had to be split into multiple k lengthed kmers that were then turned into left and right k - 1mers. I was hung up on the fact that it intuitively seems like each read should stay together, and I was trying to make just two k - 1mers from each read. One resource that helped me a lot was the de Bruijn Graph slideshow (Langmead) posted in the reading section of this module. This slideshow showed me how to create the k-mers the right way and it had a similar implmentation in code which I adapted to the specifics of this project.

That being said, my implementation was much less succesful than the one seen in our Jupyter notebook. It could almost assemble the toy reads, though the assembly was always too short. This is because it would follow the nodes to the end or until it reach a node without anymore edges, without taking every edge as required by a eulerian walk. I have been trying to use Ildiko's implementation to fix this, though I still can't figure it out. I plan to continue working on this issue to try to reach a solution. Overall, I found this project to be the hardest one yet, but I enjoyed refreshing/learning object oriented programming and recursion.   

## Other member - Ildiko Polyak
Marcus described last week's Gibbs sampling project as harder than this one, but I found graph algorithms CONSIDERABLY harder. It took me much longer to be clear on the entire process in order to write a sound pseudocode architecture for the functions. This project required recursion and object-oriented design, which I am still rather new to, and tracing calls that stack up and return in reverse order took several attempts to understand.

The bigger difference was the gap between the algorithm and a working program. In the Gibbs project, the algorithm as described was close to the code that ran. Here, the algorithm worked on the toy example, but running it on 2.4 million real reads required handling both DNA strands, false joins caused by repeats, recursion nearly 9 million calls deep, and code that was correct but too slow. Most of the work on this project was in that gap.

Once I understood the algorithm itself, I was confident in writing the pseudocode, but ultimately used Claude to help work through, understand, and fix those gaps in coding and implementation. In previous projects, I used AI very little, but for this one, I relied on it to help understand how to move from the algorithm to the working program.  This week's project taught me more through my failed code than in previous' weeks successes.

The group was cut in size from 3 to 2 members and we had conflicted schedules for the week that reduced our collaborative efforts.  I think this is a realistic real-world challenge and not necessarily a failure that gives us practice on how to move through a project in future jobs where direct collaboration can be reduced.

# Generative AI Appendix
Anthropic Claude Opus 5.5 was used for this project and many specific details are described and documented in above sections.
Pseudocode and original code was written idenpendently and original.  Claude was given the parts of the functions that were not working as anticipated and asked to explain in detail why and what the fix would be in the code.

# Additional Resources

Langmead, B. *De Bruijn Graph assembly* [PowerPoint slides]. Whiting School of Engineering, Johns Hopkins University. https://www.cs.jhu.edu/~langmea/resources/lecture_notes/17_assembly_dbg_v2.pdf
