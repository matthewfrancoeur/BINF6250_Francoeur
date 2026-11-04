# Introduction
In this project, we created a Burrows-Wheeler transformation (BWT) through two methods, first by cyclic rotations of a string and second by its suffix array. We then made use of the lexicographical ordering and last-first properties of the BWT to create pattern matching and compression/decompression algorithms. 

# Pseudocode
Put pseudocode in this box:

##initial pseudocode from my understanding of the algorithim
text_with_low_value =[]
string_text =" " and "add $ at the end
low_value_letter = "$"
if $ is not in string_text or < string text :
concatenate the two strings 
text_with_low_value.append(concatenated string)
return the concatenated string as a list
#perfom cyclic rotations to see all possible positionings of the string and store all the cyclic rotations in a list
for i in concatennated_string:
rotations = sorted(text[i:] + text[:i]
 


take the returned list and use remove to remove the last 
for i in target


generate another matrix lexicographically
remove the $ from the end and place it in front 

```
Some pseudocode here
```

## Pseudocode 

### BWT
if the end-of-string character $ is not in the input string, append it 
    create an empty list rotations
    set current_word to the string
    for each character in the string
        add  current_word to rotations 
        move the first character of current_word to the end to get the next rotation
    sort the rotations lexicographically
    initialize an empty string transformed_string
    for each rotation in rotations
        add the rotation's last character to transformed_string 
    return transformed_string

 ### suffix_array   
if the end-of-string character $ is not in the input string, append it 
    create an empty list of the suffix-position pairs
    for each position in the string
        set suffix to the slice of the string from the current position to the end
        add the (suffix, position) pair to suffix_position_pairs
    sort suffix_position_pairs lexicographically by suffix
    create an empty list positions
    for each pair in suffix_position_pairs
        add the pair's position to positions 
    return positions 

### BWT_from_suffix_array
initialize an empty string transformed_result
    for each position in  suffix_positions 
        if the position is 0 (suffix starts at the beginning of the text)
            add the last character of the text to transformed_string
        else
            add the character before the positon to transformed_string
    return the transformed_string

### cal_count
 set counts to the count of each character in the string using Counter 
    initialize an empty dictionary result_dictionary 
    set cumulative_count to zero
    for each character in the sorted uniqye characters of the string
        store cumulative_count in result_dictionary, with the character as the key
        add the character's count from counts to cumulative_count
    return result_dictionary

### cal_occur
set alphabet to the sorted unique characters in bwt_string
    initialize dictionary character_occurrences, mapping each character in the alphabet to a list of zeros the same length as bwt_string
    for each position in bwt_string
        set current_character to the character at the current position
        for each character in alphabet
            if position is greater than 0
                copy the character count from previous position into the current position
        add 1 to  current_character's count at the current position
    return character_occurrences

### update_range
if lower is 0
        set a_before to 0
    else
        set a_before to occur[a] at position lower - 1
    set new_lower to count[a] + a_before
    set new_upper to count[a] + occur[a] at position upper, minus 1
    return new_lower and new_upper

### find_match
 if the end-of-string character $ is not in the reference, append it 
    set suffix_positions to the the suffix array of the reference using suffix_array 
    set bwt to the BWT of the reference using BWT_from_suffix_array with suffix_positions
    set count to cal_count of bwt
    set occur to cal_occur of bwt
    set lower to 0 and upper to the length of bwt - 1 so the range covers all rows
    for each character in the query from right to left
        if the character is not in count:
            return an empty list
        update lower and upper using update_range with the current character
        if lower is greater than upper (empty range)
            return an empty list
    set positions_list to the entries of suffix_positions from row lower through row upper
    sort and return positions_list

### run_length_encode
initialize an empty string encoded_text
    if bwt_string is empty
        return an empty string
    set current_character to the first character of bwt_string
    set counter to 1 
    for each character from the second to the end of bwt_string
        if the character is the same as current_character
            add 1 to the counter
        else:
            add current_character and its counter to encoded_text
            set current_character to this character
            set counter to 1
    add current_character and counter to encoded_text
    return encoded_text

### run_length_decode
 initialize an empty string decoded_text 
    set position to 0
    while position is less than the length of encoded
        set symbol to the character at position
        move position forward 1
        initialize an empty string digits 
        while position is less than the length of encoded and character at position is a digit
            add the character at position to digits
            move position forward 1
        set count to digits covnerted to a number
        add count copies of symbol to decoded_text
    return decoded_text

# Dependencies
python                

# Successes
- We were able to use the walkthrough on Canvas to understand the BWT, which helped us understand what each function was for and write the pseudocode for them
- We were able to write pseudocode for every function before coding, which made writing the code much simpler
- Function outputs match expected

# Struggles
- The hardest part was understanding update_rage and the indexing for the backward search. The formulas use `occur[a][lower - 1]` and `occur[a][upper] - 1` and I needed to work through an example row by row to see why the `-1` is inside the brackets in one case but outside in another
- The second doctest for `cal_occur` appears to have a typo: it expects `'a': [0, 1, 1, 1, 1, 2, 3]` and `'n': [0, 0, 2, 2, 2, 2, 2]` but counting through each position gives `[1, 1, 1, 1, 1, 2, 3]` and `[0, 1, 2, 2, 2, 2, 2]`
- We were only able to meet synchronously once. This made it so we are all in different stages of understanding at the end of this project. 

# Personal Reflections
## Group Leader
Matthew (Jake): As expected, this project was algorithmically difficult. I spent a lot of time writing out the various steps of the project using the "banana$" example from the html (Sherman, 2025), and eventually started to get an ok understanding of what we were doing. That beign said, I still struggled with quite a few aspects conceptually. For example, why constructing a BWT from a suffix array works (not just that it does), and find_match(), update_range() functions. I do moslty get how to pattern match using pen and paper, but the code for this is still hard for me to understand.

## Other member - Katelyn
I went into this project without any knowledge of the Burrows-Wheeler transform beyond what we discussed in class, but the walkthrough was extremely helpful in making sure I understood the algorithm step by step. I was able to use the pseudocode provided in the skeleton code as a starting point to write my own pseudocode for each function, and I relied on outside help much less than in previous projects, mostly to check my steps and catch mistakes. I felt much more confident writing pseudocode than I have before. Once the pseudocode was done, the code itself was relatively simple to write, since most lines translated directly from a step that I had already planned out. 
I was out of town this weekend, so I wasn't able to meet with the group on call. We communicated through Teams instead to make sure everyone was on the same page, and each of us was still able to write out own pseudocode.  

# Generative AI Appendix
Katelyn: I used Claude Opus 5.5 throughout this project to help me use Git and GitHub. I also used it alongside the walkthrough provided to help me understand the BWT, suffix arrays, count and occurrence arrays, backward search, and run-length encoding and decoding. I wrote a first draft of the pseudocode for each function based on the pseudocode provided in the skeleton code and had Claude review each draft and point out mistakes or places where the pseudocode didn't match the skeleton. For `update_range` and `find_match` I had a harder time with starting the pseudocode, so Claude laid out the formulas and overall structure for me to reference. I wrote the code for each function from my pseudocode, and Claude reviewed it and explained and helped me correct errors. Claude cleaned up the wording of my pseudocode at the end and wrote some line comments for the function as well as drafted my Successes and Struggles and helped me edit my Personal Reflection for the README.

# References
1. Sherman, M. D. (2025). *Project 07: Burrows-Wheeler Transform* [Class handout]. Canvas@NEU. https://canvas.northeastern.edu/
