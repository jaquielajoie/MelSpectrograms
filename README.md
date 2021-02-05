# Mel-Spectrograms

- librosa seems to be a very powerful package  
<a href="https://librosa.org/doc/latest/index.html"> https://librosa.org/doc/latest/index.html </a>  
<code> pip install -y librosa </code>

1. Install
  - <code> git clone https://github.com/jaquielajoie/MelSpectrograms.git </code>
  - <code> cd MelSpectrograms </code>
  - <code> python -m venv .venv </code>
  - <code> source .venv/bin/activate </code>
  - <code> pip install -r requirements.txt </code>

2. Run JupyterLab
  - <code> jupyter-lab </code>
  - <code> localhost:8888 (default) </code>

# MFCCs (included in the notebooks)
- BER: band energy ratio (mostly pertains to music genre identification)
- SC: spectral centroid (maps onto brightness of a feature - like timbre)
  - weighted mean of frequencies (taken at a given time 't')
  - audio/music classification problems !!
- BW: bandwidth (spectral range of interest around the centroid)
  - weighted mean of distances of frequency bands from SC
  - greater bias within band, lower BW. Higher variance, greater BW.
  - music classification (human voice BW is relevant between ~190hz -> ~5000hz, phone companies filter harder than this and it is still intelligible)
