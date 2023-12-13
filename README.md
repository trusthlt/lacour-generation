# LaCour! Generation
Companion code to the [arXiv preprint](http://arxiv.org/abs/2312.05061) presenting the ``LaCour!`` corpus.
If you are looking for the dataset, please visit [LaCour! Corpus](https://github.com/trusthlt/lacour-corpus).

> [!NOTE]  
> This repo is still a work in progress and will be updated in the coming days! 

## Installation

For installation with Miniconda:

```
conda create -n lacour-generation python=3.9
conda activate lacour-generation
git clone https://github.com/trusthlt/lacour-generation.git
cd lacour-generation
pip install -r requirements.txt
```

## Running the scraper

Producing the hearing transcripts and associated documents is divided into several steps. The code for all scrapers is located in [scrape](scrape).
1. Download video files and video information by running ``scrape_webcast_videos.py``, produces ``all_webcasts_{date}.json``
2. Find associated files in HUDOC and download them by running ``scrape_case_html_matching_webcast.py``
3. Find related press releases in HUDOC and download them by running ``scrape_press_releases.py``

### Downloading the videos
> [!WARNING]  
> Due to changes to the webcast website, the scraper for videos no longer works. You can instead skip the first step and download the last scraped file ``all_webcasts.json``


## Transcribing the videos

Transcribing a video into a hearing transcript requires several steps, with one manual annotation step. The code for transcription is located in [transcribe](transcribe).
1. Diarize the video by running ``diarize.py``. This requires a huggingface token to access the models of [pyannote/speaker_diarization@2.1](https://huggingface.co/pyannote/speaker-diarization)
2. Generate a speaker schedule, clustering the diarization output by running ``generate_speaker_schedule.py``. This will result in one text file with a speaker schedule per hearing webcast
3. (MANUAL) Annotate the speaker schedule with the correct tags
4. Generate a transcript by passing the annotated speaker schedule with the video to ``transcribe_segmented_whisper.py``
