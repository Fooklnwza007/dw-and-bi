# Instruction

1 เข้า Folder Capstone-project เพื่อสร้าง project 
```sh
cd 08-Capstone-project
```

2. สร้าง Environment ในการสร้าง project python
```sh
python -m venv ENV
```

3. Activate เพื่อเข้าไปใน ENV เพื่อเก็บ package ที่ใช้งานใน project capstone นี้
```sh
source ENV/bin/activate
```

4. ในขณะที่อยู่ใน ENV เปิดใช้งาน Apache airflow port 8080
```sh
docker compose up
```

5. สร้าง Project และ key บน Google cloud เพื่อให้ Airflow สามารถเชื่อมต่อได้
จากนั้น Copy ข้อมูลจากไฟล์ JSON ทั้งหมดใส่ในช่อง Keyfile JSON กด Save




ุ6. Project Capstone นี้ จะทำงานโดยการนำข้อมูลจาก Folder Data นำเก็บเข้าใน Google storage (GCS) จากนั้นจึงนำข้อมูลใน GCS ส่งเข้าไปใน Data warehouse (Google Big query) Loop การนำเข้าข้อมูลด้วยการใช้ Airflow เป็นดังนี้
Local file > GCS > Google Big query

สร้าง code การทำ airflow บน etl.py (DummyOperator จะมีหรือไม่มีก็ได้)

6.1 การนำข้อมูลจาก Local file เข้าสู่ Google storage จะใช้ Operator = LocalFilesystemToGCSOperator (สร้างตามจำนวน Local File ที่จะนำเข้า)

6.2 การนำข้อมูลจาก Google storage เข้า Google bigquery ใช้ Operator = GCSToBigQueryOperator


6.3 ผลลัพธ์ที่ได้จากการนำเข้าข้อมูลจะมีโครงสร้าง table ดังนี้ project_id = dataengineer-415510, datasets = order, table = olist_customers_dataset, olist_geolocation_dataset, olist_order_items_dataset, olist_order_payments_dataset, olist_order_reviews_dataset, olist_orders_dataset, olist_products_dataset, olist_sellers_dataset, product_category_name_translation
จะแสดงอยู่บน data warehouse Google bigquery



7. Download library dbt-core เพื่อให้สามารถใช้งานเครื่องมือ dbt ได้
```sh
pip install dbt-core
```

ึ8. สร้าง project profile dbt ที่สร้างด้วย google bigqeury มีรายละเอียดดังนี้
```sh
dbt init
```

9. หลังจากสร้าง project profile สร้าง file ใน folder projectcapstone/models ด้วยชื่อ profiles.yml
และนำข้อมูลทั้งหมดจาก code ด้านล่าง มาใส่ไว้ใน file profiles.yml ที่เราสร้างใน projectcapstone/models 
```sh
code /home/codespace/.dbt/profiles.yml
```



10. ทดสอบการ connection กับ bigquery
```sh
dbt debug
```

12. ข้อมูล raw tables จะถูกนำมาสร้างเป็น view เพื่อดูข้อมูลตามเงื่อนไขที่ต้องการ และเพื่อป้องกันการเปลี่ยนแปลงของ raw tables จาก user 
สามารถกำหนดการสร้าง view หรือ table ได้จาก dbt_project.yml




13. จากนั้นรันคำสั่ง test เพื่อเช็ค data quality เพื่อดูเงื่อนไขการ transform ของ view/table ที่สร้างจากไฟล์ schema.yml
```sh
dbt test
```

14. การใช้เรียกดูข้อมูลด้วยการใช้ dbt จะอ่านจากไฟล์ .sql ที่อยู่ใน folder models (การทำงานไม่สนโครงสร้างของ folder)




15. ข้อมูลที่ต้องการในการสร้าง Sales report เพื่อนำไปใช้ในการวิเคราะห์
 



16. ทำ Visualization ด้วยโปรแกรม Tableau โดยการเรียกข้อมูลจาก google bigquery

