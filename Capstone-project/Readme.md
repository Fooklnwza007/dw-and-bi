# Instruction

1 เข้า Folder Capstone-project เพื่อสร้าง project 
```sh
cd 08-Capstone-project
```

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/31c6ab9e-d1cd-419b-82fe-eaf66d369a20)


2. สร้าง Environment ในการสร้าง project python
```sh
python -m venv ENV
```
![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/6625d1e8-c4df-44ef-b591-71eb85be6b98)


3. Activate เพื่อเข้าไปใน ENV เพื่อเก็บ package ที่ใช้งานใน project capstone นี้
```sh
source ENV/bin/activate
```
![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/98a0a3ea-92fc-48ff-b386-22eb35bc34dd)


4. ในขณะที่อยู่ใน ENV เปิดใช้งาน Apache airflow port 8080
```sh
docker compose up
```

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/da37729f-a949-4206-a07d-d16c675de447)


5. สร้าง Project และ key บน Google cloud เพื่อให้ Airflow สามารถเชื่อมต่อได้
จากนั้น Copy ข้อมูลจากไฟล์ JSON ทั้งหมดใส่ในช่อง Keyfile JSON กด Save

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/40befd90-c35b-40fa-bbb9-c3bd09798fff)


![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/e6550511-32b9-4d3d-9caa-a626b56508df)



6. Project Capstone นี้ จะทำงานโดยการนำข้อมูลจาก Folder Data นำเก็บเข้าใน Google storage (GCS) จากนั้นจึงนำข้อมูลใน GCS ส่งเข้าไปใน Data warehouse (Google Big query) Loop การนำเข้าข้อมูลด้วยการใช้ Airflow เป็นดังนี้
Local file > GCS > Google Big query

สร้าง code การทำ pipeline ด้วย airflow บน etl.py (DummyOperator จะมีหรือไม่มีก็ได้)

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/0685e948-0b88-45b8-aa94-bbdfac0e8d5b)

6.1 สร้าง Python Operator เพื่อนำข้อมูลจาก Local files เข้า Pipeline airflow

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/ad866792-02df-4f15-963f-8e4c7bf18f02)


6.2 การนำข้อมูลจาก Local file เข้าสู่ Google storage จะใช้ Operator = LocalFilesystemToGCSOperator (สร้างตามจำนวน Local File ที่จะนำเข้า)

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/c6179fc4-5300-4bfb-aeba-9fd85c7e620d)


6.3 การนำข้อมูลจาก Google storage เข้า Google bigquery ใช้ Operator = GCSToBigQueryOperator

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/743404e9-80f1-46dc-bd91-a25e1ea3b5a6)


6.4 ผลลัพธ์ที่ได้จากการนำเข้าข้อมูลจะมีโครงสร้าง table ดังนี้ project_id = dataengineer-415510, datasets = order, table = olist_customers_dataset, olist_geolocation_dataset, olist_order_items_dataset, olist_order_payments_dataset, olist_order_reviews_dataset, olist_orders_dataset, olist_products_dataset, olist_sellers_dataset, product_category_name_translation
จะแสดงอยู่บน data warehouse Google bigquery


![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/33a9c291-43cc-4ed1-9377-427e80552289)

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/47108aab-ce5d-4e62-a4d4-bf5ec1215d6c)


7. Download library dbt-core dbt-bigquery เพื่อให้สามารถใช้งานเครื่องมือ dbt และใช้ dbt ที่เชื่อมต่อกับ bigqueryได้
```sh
pip install dbt-core dbt-bigquery
```
![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/388c3fde-5445-43ef-9e2d-d2aaced07fa5)


8. สร้าง project profile dbt ที่สร้างด้วย google bigqeury มีรายละเอียดดังนี้
```sh
dbt init
```
![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/044f9fea-d1f0-4dc1-adb0-11b21a98a899)



9. หลังจากสร้าง project profile สร้าง file ใน folder projectcapstone/models ด้วยชื่อ profiles.yml
และนำข้อมูลทั้งหมดจาก code ด้านล่าง มาใส่ไว้ใน file profiles.yml ที่เราสร้างใน projectcapstone/models 
```sh
code /home/codespace/.dbt/profiles.yml
```

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/ba6e6e5d-aee8-4bb7-b315-1f0a352dc7e3)



10. ทดสอบการ connection กับ bigquery
```sh
dbt debug
```
![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/8a704531-70ae-45dc-83c7-76ee141d861d)



11. _src.yml ใช้ในการอ้างอิง source data (ที่อยู่บน google biquery) โครงสร้างประกอบด้วย
project_name = dataengineer-415510, schema = order (datasets), tables = olist_customers_dataset, olist_geolocation_dataset, olist_order_items_dataset, olist_order_payments_dataset, olist_order_reviews_dataset, olist_orders_dataset, olist_products_dataset, olist_sellers_dataset, product_category_name_translation

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/c1be77e6-ae62-4803-adf7-7b10191014d6)




12. ข้อมูล source tables จะถูกนำมาสร้างเป็น table หลัก เพื่อใช้ในการอ้างอิงในการสร้าง view ตามเงื่อนไขที่ต้องการ และเพื่อป้องกันการเปลี่ยนแปลงของ source tables จาก user 
สามารถกำหนดการสร้าง view หรือ table ได้จาก dbt_project.yml

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/c7dcc1c5-120f-4d29-9dc6-adf2c1528ae5)

13. การ transform source data ด้วยการใช้ dbt จะอ่านจากไฟล์ .sql ที่อยู่ใน folder models (การทำงานไม่สนโครงสร้างของ folder) และเช็คเงื่อนไขการสร้าง view/tables จากการ transform จากไฟล์ schema.yml

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/cbdd893b-b77f-4f6d-9456-82d62fcd8720)

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/000e40b6-b9c8-4f68-8ef1-e7a332ae5cc9)



14. จากนั้นรันคำสั่ง test เพื่อเช็ค data quality เพื่อดูเงื่อนไขการ transform ของ view/table ที่สร้างจากไฟล์ schema.yml โดยเงื่อนไขคือ order_id ของ olist_obt tables และ ทั้ง 3 view ต้องไม่มีค่า NULL
```sh
dbt test
```
![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/3c4e9cf3-0361-468c-90f0-62de6ec7e74e)


![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/3701ac35-855d-450b-a104-a0336c175f8c)


15. ข้อมูลที่ถูก Transform จะถูกนำขึ้น google bigquery โดยมีโครงสร้าง project_name = dataengineer-415510, schema = dbt_olist (datasets), tables = olist_obt (table), view_delivery_performance (view), view_sale_performance (view), view_seller_performance (view)
ข้อมูลที่เป็น view หรือ table ดูได้จากสัญลักษณ์ด้านหน้า

![image](https://github.com/Fooklnwza007/dw-and-bi/assets/131597296/fbe85b65-9176-4189-bc2d-5287d48648fd)


 
