# meditation_web_scrape

`webscrape.ipynb` was used to scrape the internet to collect and clean data to form the dataset `yt-transcripts-data-cleaned.csv`. The dataset contains ~5000 meditation transcripts. Each transcript comes with a meditation type that was assigned to it, that could be used for training purposes.

The notebook searched through ~300 youtube playlists, 10,000+ youtube links from those playlists and extracted ~5,000 transcripts that were used to train the text generation model for [meditation_induction_ai](https://github.com/petern48/meditation_induction_ai).

Note: Transcripts are not available for all youtube videos and many of the videos consisted of music only, which led to the decrease from 10,000 links to 5,000 actual transcripts.

### Details of Implementation
The main source of transcripts comes from Youtube. The `selenium` library was used to control a web driver to do the following:
- search for [MEDITATION TYPE] meditation youtube playlists then save their links and [MEDITATION TYPE] to `yt-playlists.csv`
- open each playlist and save the links to all the videos in the playlist to `yt-links.csv`
- Use the `youtube-transcript-api` to extract the transcripts for each of the links by inputting their links. Save these transcripts along with their [MEDITATION TYPE] and url to `yt-transcripts-data.csv`
- Clean the data by removing phrases in parentheses `()` or brackets `[]` such as '[music]'. Also replace consecutive white spaces with a single space. Remove common intro sentences that appear in many videos. Save the cleaned data to `yt-transcripts-data-cleaned.csv`

