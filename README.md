- Explore the files, structure and their dimensions (ﬁle size, number of columns, number of lines, ect...)  

`wc -l fang_et_al_genotypes.txt snp_position.txt`
`du -h fang_et_al_genotypes.txt snp_position.txt`
`awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt`
`awk -F "\t" '{print NF; exit}' snp_position.txt`  

`fang_et_al_genotypes.txt` has 2783 rows, 986 columns, size is 11M
`snp_position.txt` has 984 rows, 15columns, size is 84K

## maize data

- First, I extracted the header of the `fang_et_al_genotypes.txt` and put it in `maize_genotypes.txt`. Then, I extracted the selected maize data (rows) and append them to the same header.

`head -n 1 fang_et_al_genotypes.txt > maize_genotypes.txt`  
`grep "ZMM" fang_et_al_genotypes.txt >> maize_genotypes.txt`  

- Before joining the genotypes data with position data, I transposed `maize_genotypes.txt` with awk’s transpose function.  

`awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt`  

- After transposition, I found the transposed data has 986 rows, and the top three rows are headers that needs to be removed. I removed the top three rows and sorted the data by SNPID in the first column. There are 983 rows and 1574 columns.  

`tail -n +4 transposed_maize_genotypes.txt|sort -k1,1 > transposed_maize_genotypes_sorted.txt`  

- For the ”snp_position.txt”, I removed the header, select the 1st, 3rd and 4th column, and sorted by SNPID. There are 983 rows and 3 columns.  

`tail -n +2 snp_position.txt|cut -f 1,3,4|sort -k1,1 > snp_position_sorted.txt`  

- Once the two data are clean, I joined them by their shared column. (`join -t $’\t’` doesn’t work for me, I have to type join<Spc>'-t<Ctrl-v><Tab>’) After joining, there are 1576 columns.  

`join '-t     ' -1 1 -2 1 snp_position_sorted.txt transposed_maize_genotypes_sorted.txt > maize_sortjoined_file.txt`  

- To generate 10 ﬁles (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? This can be done by selecting rows with each chromosome number from the second column and sorted by the third column.  

`awk '$2==2’ maize_sortjoined_file.txt | sort -k3,3n > maize_chr2_inc.txt`  
`awk '$2==3' maize_sortjoined_file.txt | sort -k3,3n > maize_chr3_inc.txt`  
`awk '$2==4' maize_sortjoined_file.txt | sort -k3,3n > maize_chr4_inc.txt`  
`awk '$2==5' maize_sortjoined_file.txt | sort -k3,3n > maize_chr5_inc.txt`  
`awk '$2==6' maize_sortjoined_file.txt | sort -k3,3n > maize_chr6_inc.txt`  
`awk '$2==7' maize_sortjoined_file.txt | sort -k3,3n > maize_chr7_inc.txt`  
`awk '$2==8' maize_sortjoined_file.txt | sort -k3,3n > maize_chr8_inc.txt`  
`awk '$2==9' maize_sortjoined_file.txt | sort -k3,3n > maize_chr9_inc.txt`  
`awk '$2==10' maize_sortjoined_file.txt | sort -k3,3n > maize_chr10_inc.txt`  

- To get 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -. The intermediate files generated from the previous question can be used to sort and replace ? to -.  

`sed 's/?/-/g' maize_chr1_inc.txt | sort -k3,3nr > maize_chr1_dec.txt`  
`sed 's/?/-/g' maize_chr2_inc.txt | sort -k3,3nr > maize_chr2_dec.txt`  
`sed 's/?/-/g' maize_chr3_inc.txt | sort -k3,3nr > maize_chr3_dec.txt`  
`sed 's/?/-/g' maize_chr4_inc.txt | sort -k3,3nr > maize_chr4_dec.txt`  
`sed 's/?/-/g' maize_chr5_inc.txt | sort -k3,3nr > maize_chr5_dec.txt`  
`sed 's/?/-/g' maize_chr6_inc.txt | sort -k3,3nr > maize_chr6_dec.txt`  
`sed 's/?/-/g' maize_chr7_inc.txt | sort -k3,3nr > maize_chr7_dec.txt`  
`sed 's/?/-/g' maize_chr8_inc.txt | sort -k3,3nr > maize_chr8_dec.txt`  
`sed 's/?/-/g' maize_chr9_inc.txt | sort -k3,3nr > maize_chr9_dec.txt`  
`sed 's/?/-/g' maize_chr10_inc.txt | sort -k3,3nr > maize_chr10_dec.txt`  

## teosinte data

- Extracted teosinte data and repeated the process as described in maize  

`head -n 1 fang_et_al_genotypes.txt > teosinte_genotypes.txt`  

`grep "ZMP" fang_et_al_genotypes.txt >> teosinte_genotypes.txt`  

`awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt`  

`tail -n +4 transposed_teosinte_genotypes.txt|sort -k1,1 > transposed_teosinte_genotypes_sorted.txt`  


`join '-t     ' -1 1 -2 1 snp_position_sorted.txt transposed_teosinte_genotypes_sorted.txt > teosinte_sortjoined_file.txt`  

- To generate 10 ﬁles (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? This can be done by selecting rows with each chromosome number from the second column and sorted by the third column.   

`awk '$2==1' teosinte_sortjoined_file.txt | sort -k3,3n > teosinte_chr1_inc.txt`  
`awk '$2==2’ teosinte_sortjoined_file.txt | sort -k3,3n > teosinte_chr2_inc.txt`  
`awk '$2==3' teosinte_sortjoined_file.txt | sort -k3,3n > teosinte_chr3_inc.txt`  
`awk '$2==4' teosinte_sortjoined_file.txt | sort -k3,3n > teosinte_chr4_inc.txt`  
`awk '$2==5' teosinte_sortjoined_file.txt | sort -k3,3n > teosinte_chr5_inc.txt`  
`awk '$2==6' teosinte_sortjoined_file.txt | sort -k3,3n > teosinte_chr6_inc.txt`  
`awk '$2==7' teosinte_sortjoined_file.txt | sort -k3,3n > teosinte_chr7_inc.txt`  
`awk '$2==8' teosinte_sortjoined_file.txt | sort -k3,3n > teosinte_chr8_inc.txt`  
`awk '$2==9' teosinte_sortjoined_file.txt | sort -k3,3n > teosinte_chr9_inc.txt`  
`awk '$2==10' teosinte_sortjoined_file.txt | sort -k3,3n > teosinte_chr10_inc.txt`  

- To get 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -. The intermediate files generated from the previous question can be used to sort and replace ? to -.  

`sed 's/?/-/g' teosinte_chr1_inc.txt | sort -k3,3nr > teosinte_chr1_dec.txt`  
`sed 's/?/-/g' teosinte_chr2_inc.txt | sort -k3,3nr > teosinte_chr2_dec.txt`  
`sed 's/?/-/g' teosinte_chr3_inc.txt | sort -k3,3nr > teosinte_chr3_dec.txt`  
`sed 's/?/-/g' teosinte_chr4_inc.txt | sort -k3,3nr > teosinte_chr4_dec.txt`  
`sed 's/?/-/g' teosinte_chr5_inc.txt | sort -k3,3nr > teosinte_chr5_dec.txt`  
`sed 's/?/-/g' teosinte_chr6_inc.txt | sort -k3,3nr > teosinte_chr6_dec.txt`  
`sed 's/?/-/g' teosinte_chr7_inc.txt | sort -k3,3nr > teosinte_chr7_dec.txt`  
`sed 's/?/-/g' teosinte_chr8_inc.txt | sort -k3,3nr > teosinte_chr8_dec.txt`  
`sed 's/?/-/g' teosinte_chr9_inc.txt | sort -k3,3nr > teosinte_chr9_dec.txt`  
`sed 's/?/-/g' teosinte_chr10_inc.txt | sort -k3,3nr > teosinte_chr10_dec.txt`  
