1. when() 等于SQL case  when then 

   ```sql
   CASE
       WHEN condition1 THEN result_value1
       WHEN condition2 THEN result_value2
       -----
       -----
       ELSE result
   END;
   ```

   

   ```python
   
   from pyspark.sql.functions import when
   df2 = df.withColumn("new_gender", when(df.gender == "M","Male")
                                    .when(df.gender == "F","Female")
                                    .when(df.gender.isNull() ,"")
                                    .otherwise(df.gender))
   
   ```

   

   

2. expr() 执行sql

3.  Pivot 和 Unpivot

   

