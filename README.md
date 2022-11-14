## Testing grai with DBT and Postgres project: `jaffle_shop_grai`

`jaffle_shop` is a fictional ecommerce store invented by our friends at DBT. The original DBT reposiroty from which this is forked, transformed raw data into two models, customer and orders, ready for analytics. This repo builds on that to allow you to create a grai project with two sources from the same data, DBT and postgres. 

### What is this repo?
What this repo _is_:
- A self-contained playground grai project, focused on postgres and DBT.
- Pre requisites are Docker, Python and a spirit of adventure.

### Running this project
To get up and running with this project:
1. Spin up grai locally
```
git clone https://github.com/grai-io/grai-core
cd grai-core/grai-server
docker compose up
```

2. Navigate to a new folder and clone this repository.
```bash
$ git clone https://github.com/grai-io/jaffle_shop.git
```

3. Change into the `jaffle_shop` directory from the command line:
```bash
$ cd jaffle_shop
```

4. Create docker container for the Postgres database. Note this is setup to use port 5433 to avoid conflicts. 
```bash
$ docker-compose -f ./warehouse/docker-compose.yml up -d
```

5. Install DBT for Postgres, don't worry about configuring it, the repository includes a profiles file to work with your new local postgres database.
```bash
$ pip install dbt-postgres
```

6. Run DBT, this will build your postgres database from the jaffle shop seed files. It will also create foreign key relationships (not normally done with DBT, but essential to your production postgres!)
```bash
$ dbt build --profiles-dir ./profiles
```

7. Almost there! You now should have four docker containers running, and its time to pull it all together. Install grai CLI and the postgres and DBT connectors.
```bash
$ pip install grai-cli
$ pip install grai-source-postgres
$ pip install grai-source-dbt
```

8. Run the final script to read from DBT and postgres to populate your grai server.
```bash
$ python populate_grai.py
```

9. Checkout the results of your work! Log in to your new shiny grai frontend here: [http://localhost:3000](http://localhost:3000). Amongst other things you can see Columns and Tables as Nodes with edges reflecting connections in DBT and Foreign Keys between your tables.

Default login credentials:

```
username: null@grai.io
password: super_secret
```

Optional:
10. Run a counterfactual! Maybe we wanted to find out all of the data which would be impacted by deleting the `id` column on the `lineage_node` table.

```bash
$ python
```

```python
affected_nodes = analysis.test_delete_node(namespace='default', name='public.lineage_node.id')
for node in affected_nodes:
    print(node.spec.name)

> public.lineage_edge.source_id
> public.lineage_edge.destination_id
```

### What is a jaffle?
A jaffle is a toasted sandwich with crimped, sealed edges. Invented in Bondi in 1949, the humble jaffle is an Australian classic. The sealed edges allow jaffle-eaters to enjoy liquid fillings inside the sandwich, which reach temperatures close to the core of the earth during cooking. Often consumed at home after a night out, the most classic filling is tinned spaghetti, while my personal favourite is leftover beef stew with melted cheese.

---
For more information on dbt:
- Read the [introduction to dbt](https://docs.getdbt.com/docs/introduction).
---
