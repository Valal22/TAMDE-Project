
# Project: Theory and Applications of Model-Driven Engineering

This project of Theory and Applications of Model-Driven Engineering (TAMDE) presents five case studies where the primary goal is to define source and target metamodels, create model instances, and execute model-to-model (M2M) transformations using the Epsilon Transformation Language (ETL). The project also involves model slicing and visualization using Picto. This document provides a detailed explanation the case studies and a generalized step-by-step tutorial for setting up the environment and running the transformations in Eclipse.



## First Case Study: Farmers to Market Transformation 

This case study focuses on transforming a detailed model of farmers and their produce into a simplified model representing products available at a market.

### The Metamodels

Two distinct metamodels are defined for this transformation, representing the source and target structures.

* **Source Metamodel (A): `farmers.ecore`**. This metamodel captures information about individual farmers and the various fruits they cultivate.
    * `FarmModel`: The root container for all farmers.
    * `Farmer`: Represents a farmer with an `id`, `name`, `age`, and a list of fruits they own.
    * `Fruit`: Represents a type of produce with a `name`, `amount`, `quality`, and `price`.

* **Target Metamodel (B): `market.ecore`**. This metamodel provides a market-centric view. It represents a flattened list of products available for sale, abstracting away the individual farmer's inventory into a single market list.
    * `Market`: The root container for all product selections.
    * `Selection`: Represents a single product line for sale, containing its `name`, `quality`, `price`, `amount`, and the `farmer_id` of the producer.

### Transformation Goal

The transformation, defined in `farmer2market.etl`, reads an instance model conforming to `farmers.ecore` (i.e., a list of farmers and their fruits) and produces a new instance model conforming to `market.ecore`. The logic iterates through each farmer and each of their fruits, creating a corresponding `Selection` entry in the market model for every fruit.


## Step-by-Step MDE Tutorial 

Below is the workflow for creating metamodels, instance models, and running transformations using Eclipse and the Epsilon platform.

### 1. Project and Metamodel Setup

First, we'll set up the project structure and define the source and target metamodels.

1.  **Create Project**: In Eclipse, go to `File > New > Project...`. Select `General > Project` and give it a name (e.g., `CaseStudy1`).
2.  **Create Metamodel Folder**: Inside your new project, create a folder named `metamodel`.
3.  **Define Metamodels with Emfatic**:
    * Inside the `metamodel` folder, create two new files with the `.emf` extension (e.g., `farmers.emf` and `market.emf`).
    * Write the metamodel definitions in these files. Ensure the `@namespace` URI is unique for each metamodel.
4.  **Generate Ecore Metamodels**:
    * Right-click on each `.emf` file.
    * Select `Epsilon > Generate Ecore from Emfatic`. This will create the corresponding `.ecore` files (e.g., `farmers.ecore`, `market.ecore`), which are the standard format for EMF metamodels.
5.  **Validate Metamodels**: Right-click on each generated `.ecore` file and select `Validate`. This checks for any errors in the metamodel structure.

### 2. Creating an Instance Model

Now, we create a concrete data model (an instance) that conforms to our source metamodel.

1.  **Create Models Folder**: Create a new folder named `models` in your project root.
2.  **Define Model with Flexmi**:
    * Inside the `models` folder, create a new file with the `.flexmi` extension (e.g., `farmers_large.flexmi`). Flexmi is a user-friendly XML-based syntax for creating model instances.
    * Write the instance data in this file. Remember to reference the correct namespace URI from your source metamodel (e.g., `<?nsuri farm01?>`).
3.  **Generate XMI Model**:
    * Right-click on the `.flexmi` file.
    * Select `Generate XMI Model`. This will create an `.xmi` file (e.g., `farmers_large.xmi`), which is the instance model that the transformation engine will read.

### 3. ETL Transformation

Here, we define the transformation logic and configure the execution environment.

1.  **Create Transformation Folder**: Create a folder named `transformations` in your project.

2.  **Create ETL File**: Inside the `transformations` folder, create a new file with the `.etl` extension (e.g., `farmer2market.etl`). This file will contain the rules for transforming the source model to the target model.

3.  **Set Up Run Configuration**:

    * Go to `Run > Run Configurations...`.
    * Right-click on `ETL Transformation` and select `New Configuration`.
    * In the `ETL file` field, browse to your `.etl` file.
    * Go to the **Models** tab to configure the input and output.

4.  **Configure Source Model (Input)**:

    * Click `Add`.
    * **Name** and **Aliases**: Give it a name that matches the alias used in your `.etl` file (e.g., `farmers`).
    * **Model type**: Select `EMF Model`.
    * **Model File**: Browse to the `.xmi` instance model you generated earlier (e.g., `/CaseStudy1/models/farmers_large.xmi`).
    * **Metamodel**: Click `Add File` and select the source `.ecore` metamodel (e.g., `farmers.ecore`).
    * **Options**: Check **Read on load** and uncheck **Store on disposal**. This tells Epsilon to load this model as a read-only input.

5.  **Configure Target Model (Output)**:

    * Click `Add` again.
    * **Name** and **Aliases**: Give it a name that matches the alias used in your `.etl` file (e.g., `market`).
    * **Model type**: Select `EMF Model`.
    * **Model File**: Type the path for the output file that will be created (e.g., `/CaseStudy1/models/market_output.xmi`). **This file does not exist yet.**
    * **Metamodel**: Click `Add File` and select the target `.ecore` metamodel (e.g., `market.ecore`).
    * **Options**: Uncheck **Read on load** and check **Store on disposal**. This configures the model as a writeable output that will be saved.

6.  **Run the Transformation**: Click `Apply` and then `Run`. The transformation will execute, and the new output `.xmi` file should appear in your `models` folder.

### 4. Visualizing Metamodels with Picto

Picto can be used to generate class diagrams from your `.ecore` metamodels. Here you can go through the installation tutorial for picto (it should be read before continuing). 

1.  **Setup**:
    * Create a `picto` folder in your project.
    * From your Picto installation directory, copy the `.settings` folder, the `picto` folder, and the `.project` file into your project's `picto` folder.
2.  **Create Picto Configuration**:
    * Inside your `picto` folder, create a new file named after your metamodel with a `.picto` extension (e.g., `farmers.ecore.picto`).
    * Paste the following configuration into the file:
    ```xml
    <?nsuri picto?>
    <picto format="plantuml" transformation="picto/ecore2plantuml/ecore2plantuml.egl">
    </picto>
    ```
    * Note: For `.model` files (e.g., `market.ecore.model`), you must use the special configuration provided in the tutorial's `.model.picto` file instead of the snippet above.
3.  **Generate Diagram**: Right-click on the `.picto` file and select `Picto > Generate diagram`. This will generate a PlantUML diagram of your metamodel.


### 5. Slicing/semantic importance
