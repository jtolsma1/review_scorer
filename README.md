# Yelp Restaurant Review Scorer

This code notebook processes Yelp restaurant reviews with a **locally hosted LLM** to extract **aspect-based emotions**:<br><br>
<img src = "misc/output_sample.png" format = "png" width = "1000"></img><br>

* **Reviewed aspects:** food, service, and ambiance
* **Possible emotions:** positive, disappointed, and angry (or none, where the reviewer does not remark)

### Why process Yelp review text using an LLM?
Yelp could build an 'emotion selector' step for each review, but LLM-based characterization is better because:

* Forced data input is inconsistent and unreliable; a reviewer might input 5 stars but express mixed feelings in text
* Additional steps add friction that could prevent review completion
* Reviewers are free to say what they think without having to conform to review frameworks


## Workflow Steps

- **Data preprocessing**  
  - Subsamples the Yelp dataset (>5 GB) into a manageable working set.  
  - Cleans review text (ampersands → “and”, escape characters, accented letters).  

- **LLM classification pipeline**  
  - Builds a reusable prompt and sends batches of reviews to the LLM.  
  - Parses messy JSON-like output into structured DataFrames.  
  - Supports **reprocessing**: when invalid outputs occur, reviews are reprocessed.

- **Visualization (stacked bar charts)**  
  - Aggregates reviews by emotional response to each aspect, per restaurant.
  - Shows a prototype visualization that could be published to Yelp's website.

- **Quality monitoring**  
  - Tracks reprocessing statistics to measure LLM reliability.  
  - In testing, < 1% of outputs required retries.

## Real Model Outputs
The model output helps users to select restaurants for their needs and set their expectations:

* **Bourbon & Branch**<br>
Consider this upscale bar for a fancy date or formal event afterparty<br>
<img src = "misc/bourbon_branch.png" format = "png" width = "500"></img><br><br>

* **Sher-e-Punjab**<br>
Bring your family for a casual evening of amazing cuisine and unpretentous atmosphere<br>
<img src = "misc/sher-e-punjab.png" format = "png" width = "500"></img><br><br>

* **Copabanana**<br>
If you're going to go here, be prepared to exercise patience with the staff<br>
<img src = "misc/copabanana.png" format = "png" width = "500"></img><br><br>

* **Reprocessing Statistics**<br>
  - A common LLM failure mode is selecting an emotion not requested by the prompt (examples include "misled" and "bland")
  - These failures are handled in the reprocessing workflow
  - Reprocessing statistics show how successful the LLM is at creating the asked-for outputs<br>
<img src = "misc/reprocess_stats.png" format = "png" width = "650"></img><br><br>


## Installation & Setup

### 1. Install Ollama
This notebook uses a local instance of an LLM, which requires Ollama. Download and install Ollama for your OS:

- **macOS / Windows** → [https://ollama.ai](https://ollama.ai)  
- **Linux** →  
    ```
    bash
    curl -fsSL https://ollama.com/install.sh | sh
    ```

### 2. Pull a model

  ```
  bash
  ollama pull mistral
  ```

(Default: `mistral`, but `llama2` and others are supported.)

### 3. Clone this repo
  ```
  bash
  git clone https://github.com/your-username/review-scorer.git
  cd review-scorer
  ```

### 4. Create a virtual environment
  ```
  bash
  python3 -m venv .venv
  source .venv/bin/activate  # Windows: .venv\Scripts\activate
  pip install -r requirements.txt
  ```

### 5. (Optional) Download the Yelp Open Dataset
* If you like, download the complete [Yelp Open Dataset](https://business.yelp.com/data/resources/open-dataset/)
* This is optional because an extract of the dataset has been provided for you in `dataset/` within this repo

## Usage

1. **Start Ollama** in a separate terminal:

   ```
   bash
   ollama serve
   ```

2. **Open the notebook**:

   ```
   bash
   jupyter lab review_scorer.ipynb
   ```

3. **Execute the notebook** to process and visualize review content


## Project Structure

```
review-scorer/
│── dataset/                # Yelp dataset sample
│── review_scorer.ipynb     # Analytics notebook
│── requirements.txt        # Python dependencies
│── README.md               # This file
```

## License

MIT License — see [LICENSE](LICENSE).

