# MelSpectrograms

- librosa seems to be a very powerful package  
<a href="https://librosa.org/doc/latest/index.html"> https://librosa.org/doc/latest/index.html </a>
<code> pip install -y librosa </code>

1. Install
- git clone https://github.com/jaquielajoie/MelSpectrograms.git
- cd MelSpectrograms
- python -m venv .venv
- source .venv/bin/activate
- pip install -r requirements.txt

2. Run JupyterLab
- jupyter-lab
- localhost:8888 (default)

#MFCCs
- BER: band energy ratio (mostly pertains to music genre identification)
- SC: spectral centroid (maps onto brightness of a feature - like timbre)
  - weighted mean of frequencies (taken at a given time 't')
  - audio/music classification problems !!
- BW: bandwidth (spectral range of interest around the centroid)
  - weighted mean of distances of frequency bands from SC
  - greater bias within band, lower BW. Higher variance, greater BW.
  - music classification (human voice BW is relevant between ~190hz -> ~5000hz, phone companies filter harder than this and it is still intelligible)
