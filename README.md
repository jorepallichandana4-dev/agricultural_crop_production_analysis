# Agricultural Crop Production Analysis System

Beginner-friendly Python + MySQL + Pandas + NumPy + Matplotlib + Streamlit project.

## Units
- cultivated_area: hectares
- production: tonnes
- rainfall: millimetres (mm)
- calculated yield: tonnes per hectare

The included dataset is synthetic and is only for testing the software. It is not real historical agricultural data.

## Setup
1. Install Python, MySQL Server, MySQL Workbench, and PyCharm.
2. In the PyCharm terminal run: `pip install -r requirements.txt`
3. Open `create_tables.sql` in MySQL Workbench and execute it.
4. In `database.py`, set the MySQL password. Better: set an environment variable named `MYSQL_PASSWORD`.
5. Run `python generate_data.py`
6. Run `python validation.py`
7. Run `python load_data.py`
8. Run `python analysis.py`
9. Run `streamlit run app.py`

## Useful SQL commands
```sql
use agricultural_crop_db;
show tables;
select count(*) from crop_records;

select c.crop_name, sum(cr.production) as total_production_tonnes
from crop_records cr
join crops c on cr.crop_id = c.crop_id
group by c.crop_name
order by total_production_tonnes desc;

select r.region_name, sum(cr.production) as total_production_tonnes
from crop_records cr
join regions r on cr.region_id = r.region_id
group by r.region_name
order by total_production_tonnes desc;

select year, sum(production) as total_production_tonnes
from crop_records
group by year
order by year;

select c.crop_name, r.region_name, cr.year,
       cr.cultivated_area, cr.production,
       case
           when cr.cultivated_area > 0 and cr.production is not null
           then cr.production / cr.cultivated_area
           else null
       end as yield_tonnes_per_hectare
from crop_records cr
join crops c on cr.crop_id = c.crop_id
join regions r on cr.region_id = r.region_id
order by cr.year, c.crop_name, r.region_name;
```

## Important notes
- The loader clears existing rows before importing the included sample data. Do not run it against a database containing data you want to preserve.
- Zero or missing cultivated area results in unavailable yield.
- Review missing and invalid data before interpreting the summaries.
- Comparisons describe associations and historical patterns, not causation.
