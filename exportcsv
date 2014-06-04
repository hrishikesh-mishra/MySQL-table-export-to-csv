#!/bin/bash
# 
# Export table data to csv format including headers 
# Author: Hrishikesh Mishra
#  
#


if [ "$#" -lt 1 ]
then
  echo "Usage: exportcsv <table_name> <year>"
  exit 1
fi


DBNAME='<db_name>'
DBUSER='<db_username>'
DBPWD='<db_pwd>'
TABLE=$1
YEAR_CONDITION=''
YEAR_CONDITION_FILE='-All-'

if [ "$2" != "" ] 
then 
YEAR_CONDITION="  WHERE year(created_at) = $2 "
YEAR_CONDITION_FILE="-Year_$2-"
fi 

FNAME=/tmp/$DBNAME-$TABLE-$YEAR_CONDITION_FILE-$(date +%Y.%m.%d).csv
TEMP_FILE='/tmp/tempfile.csv'


if [ -f $TEMP_FILE ];
then
   echo "First remove the temporary file:  $TEMP_FILE"
   exit 2
fi 

mysql -u $DBUSER -p $DBPWD $DBNAME -B -e "SELECT COLUMN_NAME FROM information_schema.COLUMNS C WHERE table_name = '$TABLE' AND TABLE_SCHEMA = '$DBNAME' ;" | awk '{print $1}' | grep -iv ^COLUMN_NAME$ | sed 's/^/"/g;s/$/"/g' | tr '\n' ';' > $FNAME
echo "" >> $FNAME
mysql -u $DBUSER -p $DBPWD $DBNAME -B -e "SELECT * FROM $TABLE  $YEAR_CONDITION  INTO OUTFILE '$TEMP_FILE' FIELDS TERMINATED BY ';'   ENCLOSED BY '\"'  ESCAPED BY '\\\'  LINES TERMINATED BY '\n' ;"
cat $TEMP_FILE >> $FNAME
sudo rm -rf $TEMP_FILE
echo "\nFile Create :  $FNAME"
echo "\n*************Done****************"  












