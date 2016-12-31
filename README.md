# mongo-import-export
Import and export mongodb databases

## Export
```{r, engine='sh', count_lines}
#!/bin/bash

if [ ! $1 ]; then
        echo " Example of use: $0 databasename [dir_to_store]"
        exit 1
fi
db=$1
out_dir=$2
if [ ! $out_dir ]; then
        out_dir="./"
else
        mkdir -p $out_dir
fi

tmp_file="fadlfhsdofheinwvw.js"
echo "print('_ ' + db.getCollectionNames())" > $tmp_file
cols=`mongo $db $tmp_file | grep '_' | awk '{print $2}' | tr ',' ' '`
for c in $cols
do
    mongoexport -d $db -c $c -o "$out_dir/${c}.json"
done
rm $tmp_file

```

## Import
```{r, engine='bash', count_lines}
#!/bin/bash

if [ ! $1 ]; then
        echo " Example of use: $0 databasename [data_directory/]"
        exit 1
fi
db=$1
out_dir=$2
if [ ! $out_dir ]; then
        out_dir="./"
else
        mkdir -p $out_dir
fi


for file in $out_dir*.json; 
do 
    mongoimport --db $db --collection "" --file "${file}"; 
done


```
