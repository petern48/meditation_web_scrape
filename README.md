# meditation_web_scrape

`webscrape.ipynb` was used to scrape the internet to collect and clean data to form the dataset `med-transcript-dataset`. The dataset contains ~5000 meditation transcripts. Most of the transcripts comes with a meditation type that was assigned to it, that could be used for training purposes.

The notebook searched through ~300 youtube playlists, extracting 10,000+ youtube links from those playlists which resulted in ~5,000 transcripts. It also searched through 10,000+ links from Insight Timer, and extracted roughly 30 transcripts from it (most of the transcripts were in video form and did not have text transcripts). These ~5000 transcripts were used to train a text generation model for [meditation_induction_ai](https://github.com/petern48/meditation_induction_ai). After review, we decided not to use this finetuned model for the in the final project, but the finetuned model can still be queried here on this repo. The current finetuned Hugging Face model can be found at [petern48/gpt2-meditation](https://huggingface.co/petern48/gpt2-meditation)

Note: Transcripts are not available for all youtube videos and many of the videos consisted of music only, which led to the decrease from 10,000 links to 5,000 actual transcripts.

### Details of Implementation
The main source of transcripts comes from Youtube. The `selenium` library was used to control a web driver to do the following:
- search for [MEDITATION TYPE] meditation youtube playlists then save their links and [MEDITATION TYPE] to `yt-playlists.csv`
- open each playlist and save the links to all the videos in the playlist to `yt-links.csv`
- Use the `youtube-transcript-api` to extract the transcripts for each of the links by inputting their links. Save these transcripts along with their [MEDITATION TYPE] and url to `yt-transcripts-data.csv`
- Clean the data by removing phrases in parentheses `()` or brackets `[]` such as '[music]'. Also replace consecutive white spaces with a single space. Remove common intro sentences that appear in many videos. Save the cleaned data to `yt-transcripts-data-cleaned.csv`

A similar process was performed on other websites but they only produced ~40 transcripts. These are stored at the top of the `med-transcript-dataset.csv`. These non-youtube transcripts have `nan` values for the all of URLs and for some of the meditation types because these were scraped first and this information was not saved for them. However, the youtube data all have an assigned med_type, URL, and transcript.
