# meditation_web_scrape

Files:
- `webscrape.ipynb` - A notebook using [Selenium WebDriver](https://selenium-python.readthedocs.io/getting-started.html) and the [YouTube Transcript API](https://pypi.org/project/youtube-transcript-api/) to extract data from YouTube videos and the Insight Timer website (implementation described below).
- `data-clean.ipynb` - A notebook used to analyze and clean the meditation transcript data. It adds punctuation and removes imperfections such as extra white spaces, words in parentheses, YouTube introductions, and other random words commonly in YouTube transcripts. Lastly, it adds special tokens to the dataset representing the meditation types.
- `GPT_2_Fine_Tuning_w_Hugging_Face_%26_PyTorch.ipynb` A notebook I used to finetune the LLM. It mostly uses borrowed code, but I did add special tokens for each meditation type for training the model.
- `text_generation_model.ipynb` is a notebook you can you to run the finetuned model. For comparison, It also contains outputs from both the base model and my model.
- `temp_files/` is a directory that contains the files that these notebooks used in the process to obtain the final dataset `med-transcript-dataset.csv`.
- `tokenized-dataset.csv` - is a dataset with added special tokens representing the meditation types.

This repo was used to scrape the internet to collect and clean data and form a the dataset `med-transcript-dataset.csv`. The dataset contains ~5000 meditation transcripts. Most of the transcripts come with a meditation type that was assigned to it, that could be used for training purposes.

The notebook searched through ~300 YouTube playlists, extracting 10,000+ YouTube links from those playlists which resulted in ~5,000 transcripts. It also searched through 10,000+ links from Insight Timer, and extracted roughly 30 transcripts from it (most of the transcripts were in video form and did not have text transcripts). These ~5000 transcripts were used to train a text generation model for [meditation_induction_ai](https://github.com/petern48/meditation_induction_ai). After review, I decided not to use this finetuned model in the final project, but the finetuned model can still be queried here on this repo using `text_generation_model.ipynb` which also contains sample outputs from both our model and the base model. The current finetuned Hugging Face model can be found at [petern48/gpt2-meditation](https://huggingface.co/petern48/gpt2-meditation). 

Note: Transcripts were not available for all YouTube videos and many of the videos consisted of music only, which led to the decrease from 10,000 links to 5,000 actual transcripts.

### Details of Implementation
The main source of transcripts comes from YouTube. The `selenium` library was used to control a web driver to do the following:
- search for [MEDITATION TYPE] meditation YouTube playlists then save their links and [MEDITATION TYPE] to `yt-playlists.csv`
- open each playlist and save the links to all the videos in the playlist to `yt-links.csv`
- Use the `youtube-transcript-api` to extract the transcripts for each of the links by inputting their links. Save these transcripts along with their [MEDITATION TYPE] and url to `yt-transcripts-data.csv`
- Clean the data by removing phrases in parentheses `()` or brackets `[]` such as '[music]'. Also, replace consecutive white spaces with a single space. Remove common intro sentences that appear in many videos. Save the cleaned data to `yt-transcripts-data-cleaned.csv`

A similar process was performed on other websites but they only produced ~40 transcripts. These are stored at the top of the `med-transcript-dataset.csv`. These non-YouTube transcripts have `nan` values for the all of URLs and for some of the meditation types because these were scraped first and this information was not saved for them. However, the YouTube data all have an assigned med_type, URL, and transcript.
