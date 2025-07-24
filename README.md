# Project: Theory and Applications of Model-Driven Engineering

This project of Theory and Applications of Model-Driven Engineering (TAMDE) presents five case studies where the primary goal is to define source and target metamodels, create model instances, and execute model-to-model (M2M) transformations using the Epsilon Transformation Language (ETL). The project also involves model slicing with specific semantic importance and visualization using Picto. This document provides an explanation of the case studies and a generalized step-by-step tutorial for setting up the environment and running the transformations in Eclipse.


# Case studies
### First Case Study: Farmers to Market Transformation 

This case study focuses on transforming a model of farmers and their produce into a simplified model representing products available at a market.

#### The Metamodels

Two metamodels are defined for this transformation, representing the source and target structures.

* **Source Metamodel (A): `farmers.ecore`** captures information about individual farmers and the fruits cultivated.
    * `FarmModel`: The root container for all farmers.
    * `Farmer`: Represents a farmer with an `id`, `name`, `age`, and a list of fruits they own.
    * `Fruit`: Represents a type of produce with a `name`, `amount`, `quality`, and `price`.

* **Target Metamodel (B): `market.ecore`** represents a flattened list of products available at the market.
    * `Market`: The root container for all product selections.
    * `Selection`: Represents a single product line for sale, containing its `name`, `quality`, `price`, `amount`, and the `farmer_id` of the producer.

#### Transformation Goal

The transformation, defined in `farmer2market.etl`, reads an instance model conforming to `farmers.ecore` (i.e., a list of farmers and their fruits) and produces a new instance model conforming to `market.ecore`. The logic iterates through each farmer and each of their fruits, creating a corresponding `Selection` entry in the market model for every fruit.


### Second Case Study: Customer to Data Warehouse Transformation 

In this case study is presented a transactional model of customer purchases which is transformed into an aggregated model suitable for a data warehouse.

#### The Metamodels

Two distinct metamodels are defined, representing the raw transactional data and the summarized analytical data.

* **Source Metamodel (A): `customer.ecore`** captures detailed information about individual customers and their specific purchase histories.
    * `CustomerModel`: The root container for all customers.
    * `Customer`: Represents a customer with a `customer_id`, `name`, `email`, `age`, and a list of their purchases.
    * `Purchase`: Represents a single transaction with a `product` name, `quantity`, `price`, and `date`.

* **Target Metamodel (B): `warehouse.ecore`** provides an aggregated view summarizing each customer's activity into a single record.
    * `DataWarehouse`: The root container for all customer facts.
    * `CustomerFact`: Represents a summarized view of a customer, containing their `customer_id`, `name`, `email`, along with calculated fields like `total_spent`, `total_orders`, and the `last_purchase_date`.

#### Transformation Goal

The transformation, defined in `customer2warehouse.etl`, reads an instance model conforming to `customer.ecore` and produces a new instance model conforming to `warehouse.ecore`. The logic iterates through each customer and aggregates their purchase history by calculating the total money spent, counting the number of orders, and identifying the most recent purchase date. 

## Step-by-Step MDE Tutorial 

Below is the workflow for creating metamodels, instance models, and running transformations using Eclipse platform.

### 1. Project and Metamodel Setup

First, we set up the project structure and define the source and target metamodels.

1.  **Create Project**: In Eclipse, go to `File > New > Project...`. Select `General > Project` and give it a name (e.g., `CaseStudy1`).
2.  **Create Metamodel Folder**: Inside the new project, create a folder named `metamodel`.
3.  **Define Metamodels with Emfatic**:
    * Inside the `metamodel` folder, create two new files with the `.emf` extension (e.g., `farmers.emf` and `market.emf`).
    * Write the metamodel definitions in these files. Ensure the `@namespace` URI is unique for each metamodel.
4.  **Generate Ecore Metamodels**:
    * Right-click on each `.emf` file.
    * Select `register Epackages` > then again right-click on each of them > `Generate Ecore`. This will create the corresponding `.ecore` files (e.g., `farmers.ecore`, `market.ecore`), which are the standard format for EMF metamodels.
5.  **Validate Metamodels**: Right-click on each generated `.ecore` file and select `Validate` (in the Editor window  -i.e. the window in the "middle"-). 

### 2. Creating an Instance Model

Now, we create a concrete data model (an instance) that conforms to our source metamodel.

1.  **Create Models Folder**: Create a new folder named `models` in the project root.
2.  **Define Model with Flexmi**:
    * Inside the `models` folder, create a new file with the `.flexmi` extension (e.g., `farmers_large.flexmi`). 
    * Write the instance data in this file. Remember to reference the correct namespace URI from the source metamodel (e.g., `<?nsuri farm01?>`).
3.  **Generate XMI Model**:
    * Right-click on the `.flexmi` file.
    * Select `Generate XMI Model`. This will create an `.xmi` file (e.g., `farmers_large.xmi`), which is the instance model that the transformation engine will read.

### 3. ETL Transformation

Here, we define the transformation logic and configure the execution environment.

1.  **Create Transformation Folder**: Create a folder named `transformations` in the project.

2.  **Create ETL File**: Inside the `transformations` folder, create a new file with the `.etl` extension (e.g., `farmer2market.etl`). This file will contain the rules for transforming the source model to the target model.

3.  **Set Up Run Configuration**:

    * Go to `Run > Run Configurations...`.
    * Right-click on `ETL Transformation` and select `New Configuration`.
    * In the `ETL file` field, go to the **Models** tab to configure the input and output.

4.  **Configure Source Model (Input)**:
    * Click `Add` > `EMF Model`.
    * **Name** and **Aliases**: Give it a name that matches the alias used in the `.emf` file (e.g., `farmers`).
    * **Model File**: Browse to the `.xmi` instance model you generated earlier (e.g., `/CaseStudy1/models/farmers_large.xmi`).
    * **Metamodel**: Click `Add File` and select the source `.ecore` metamodel (e.g., `farmers.ecore`).
    * **Options**: Check **Read on load** and uncheck **Store on disposal**. This tells Epsilon to load this model as a read-only input.

5.  **Configure Target Model (Output)**:
    * Click `Add` > `EMF Model`.
    * **Name** and **Aliases**: Give it a name that matches the alias used in the `.emf` file (e.g., `market`).
    * **Model File**: Type the path for the output file that will be created (e.g., `/CaseStudy1/models/market_output.xmi`). **This file does not exist yet.**
    * **Metamodel**: Click `Add File` and select the target `.ecore` metamodel (e.g., `market.ecore`).
    * **Options**: Uncheck **Read on load** and check **Store on disposal**. This configures the model as a writeable output that will be saved.

6.  **Run the Transformation**: Click `Ok` (for both files of 4. and 5. points) and then `Run`. The transformation will execute, and the new output `.xmi` file should appear in the `models` folder.

### 4. Visualizing Metamodels with Picto

Picto can be used to generate class diagrams from the `.ecore` metamodels. [Here](https://github.com/alfonsodelavega/picto-objectdiagram) you can go through the installation tutorial for picto (it should be read before continuing). 

1.  **Setup**:
    * Create a `picto` folder in the project.
    * From the Picto installation directory (picto-objectdiagram-main), copy the `.settings` folder, the `picto` folder, and the `.project` file into the project's `picto` folder.
2.  **Create Picto Configuration**:
    * Inside the `picto` folder, create a new file named after the metamodel with a `.picto` extension (e.g., `farmers.ecore.picto`).
    * Paste the following configuration into the file:
    ```xml
    <?nsuri picto?>
    <picto format="plantuml" transformation="picto/ecore2plantuml/ecore2plantuml.egl">
    </picto>
    ```
    * Note: For `.model` files (e.g., `market.model`), you must use the special configuration provided in the tutorial's `.model.picto` file instead of the snippet above:
    ```xml
    ?nsuri picto?>
    <picto format="plantuml" transformation="picto/model2plantuml/model2plantuml.egl">
    </picto>
    ```
3.  **Visualize Diagrams**: Double click on the `.picto` files generated.

### 5. Slicing / Semantic Importance

A **model slice** consists of selecting only the instances of *semantically important* meta‑classes. Slices help reason about transformations by isolating the pieces of information that matter for a given analysis.

**How to create a slice**

1. **Select important meta‑classes** in the *source* metamodel.
2. **Project the instance model** onto those meta‑classes: keep all their instances and hide the rest, or render them with low opacity.
3. **Repeat for different sizes** (small / medium / large) to show scalability.
4. **Identify the corresponding elements in the *target* model**: for every source instance in the slice, collect the elements produced by the transformation.

**Example – Case Study 1 (Farmers → Market)**
Semantic importance is attached to the meta‑class `Farmer`. The three screenshots show slices of the small, medium, and large source models where only the `FarmModel` root and all `Farmer` instances are highlighted; `Fruit` instances are rendered with low opacity.
