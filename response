DECLARE
  
 WORD VARCHAR2 (100) := 'Test for Jean Marie';
 
BEGIN

  DECLARE
      
        ENTERPRISE	VARCHAR2 (100) :=  WORD || ' in EMAGINE!'; 
  BEGIN
    
	DBMS_OUTPUT.put_line (ENTERPRISE);
	
	---insert into the RESULTS table each customer id, and the number of unique products purchased by that customer.
    INSERT INTO RESULTS (customer_id,product_count)
    SELECT customer_id,COUNT(product_id) product_count 
    FROM (
             SELECT distinct customer_id,product_id
             FROM CUST_PRODUCTS
         )
    GROUP BY customer_id;
	COMMIT;
    ---update the recently_purchased column of the customer table 

    MERGE INTO CUSTOMER B
    USING ( 
                SELECT customer_id, CASE WHEN ROUND(MONTHS_BETWEEN (SYSDATE,TO_DATE(date_purchased,'MM-DD-YYYY'))) <= 12 THEN 'Y' ELSE 'N' END date_purchased
				FROM CUST_PRODUCTS               
           ) T
    ON( B.customer_id= T.customer_id) 
    WHEN MATCHED THEN UPDATE SET B.recently_purchased=T.date_purchased;
	COMMIT;	
	
  END;
EXCEPTION
  WHEN OTHERS
  THEN
    DBMS_OUTPUT.put_line 
   (DBMS_UTILITY.format_error_stack);
END;
