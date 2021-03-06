#eclat
This code integrates eclat, charm and genmax into one. Note, this is
just a clean version of the individual programs, and as such not
optimal. For instance the original code for Charm and GenMax are
faster that this clean version. Also the genmax implementation used
here doesn't use all the optimizations described in the
paper. Therefore use this code only for application studies. If you
want to do performance study to compare the algorithm timings, then
please ask for the original code from me.

    RUN IT AS:
        eclat -i XXX -a <ALG_TYPE> -d <DIFF_TYPE> -s <MINSUP> -o

        other flags
         -o output the patterns found
                output format: itemset - sup (tidset)

         -b (for binary input file)

         -a 0 eclat (all frequent sets)
               this is the default algorithm
         -a 1 charm (all closed sets)
         -a 2 genmax (all maximal sets)

         -d 0 uses tidsets
         -d 1 uses diffsets instead of tidsets (from length 3 onwards)
              this is the default value
         -d 2 uses diffsets for pass 2 as well
                (this should NOT be used for sparse datasets, since tidset
                 size of pass 2 is smaller than diffset size for
                 sparse sets.)

         -c 0 do not eliminate non-closed sets (if -a 1 is used)
         -c 1 eliminate non-closed sets (if -a 1 is used)

         -S absolute MINSUP (not as a percentage)
         -x No offset (omit CID TID)
         
MINSUP is in fractions, i.e., specify 0.5 if you want 50% minsup or
0.01 if you want 1% support. You can also use -S with an absolute
minimum support, e.g., -S 2 would mean that the minimum support is 2 (as opposed to a percentage).

The input database is assumed to be ascii (use -b for binary format), with two input formats. 

The IBM datagen format is

        # CID TID #ITEMS LIST_OF_ITEMS
        1   1   4       0 1 4 6
        2   2   3       4 7 9
items in the list must be sorted in increasing order, one per line (omit
the header)

The NO OFFSET format is:

        # LIST_OF_ITEMS
         0 1 4 6
         4 7 9
items in the list must be sorted in increasing order (omit the header).
To run a file in the NO Offset format use the -x flag.


Finally the summary of the run is stored in the summary.out
file. The format of this file is as follows:

(parameter options) DB_FILENAME MINSUP NUMTRANS_IN_DB ACTUAL_SUPPORT
      [ ITER_i |Ci| |Fi| |Max| XXX] 
      [TOT total_cands tot_freq total_max xxx] TOT-TIME NUMJOINS

I have provided a utility that converts from the ASCII IBM data format
to the binary format. Use it as: makebin XXX.ascii XXX.data

A typical run can be something like:
./eclat -i T10I4D100K.data -S 1000 -a 1 -o > SAVEFILE
where the output will be written to SAVEFILE

#sort
Once you have saved the output, you can sort the patterns by increasing
length. After some meta-info, the sorted output has one pattern per
line, along with its frequency, in the format:
ITEMSET - SUPPORT

An example run can be:
./sort SAVEFILE > SORTEDFILE
