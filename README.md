# Mel-Spectrograms

1. Install
  - <code> git clone https://github.com/jaquielajoie/MelSpectrograms.git </code>
  - <code> cd MelSpectrograms </code>
  - <code> python -m venv .venv </code>
  - <code> source .venv/bin/activate </code>
  - <code> pip install -r requirements.txt </code>

2. Run JupyterLab
  - <code> jupyter-lab </code>
  - <code> localhost:8888 (default) </code>

# Included audio feature expositions in notebooks...
  - BER: band energy ratio (mostly pertains to music genre identification)
  - SC: spectral centroid (maps onto brightness of a feature - like timbre)
    - weighted mean of frequencies (taken at a given time 't')
    - audio/music classification problems !!
  - BW: bandwidth (spectral range of interest around the centroid)
    - weighted mean of distances of frequency bands from SC
    - greater bias within band, lower BW. Higher variance, greater BW.
    - music classification (human voice BW is relevant between ~190hz -> ~5000hz, phone companies filter harder than this and it is still intelligible)

# Speech formalization

(if you see this elsewhere)  
x/X = speech signal  
e/E = glottal pulse  
h/H = vocal tract freq response  
IDFT = (inverse) Discrete Fourier Transform

<i> pseudo code </i>
- Speech = convolution of vocal tract frequency response (const based on biology?) with glottal pulse.
  - speech_signal(t) = glottal_pulse(t) * vocal_tract_freq_response(t)
  - fourierTransform = (speech_signal, glottal_pulse, vocal_tract_freq_response) => { speech_signal_spectrum(t) = glottal_pulse_spectrum(t) *   vocal_tract_freq_response_spectrum(t) }
  - __log = (...) => { log(speech_signal_spectrum(t)) = log(glottal_pulse_spectrum(t)) + log(vocal_tract_freq_response_spectrum(t)) }
    - we can now treat glottal_pulse_spectrum(t) as separate from vocal_tract_freq_response_spectrum(t)

- glottal pulse is not that important and occupies a higher quefrency.
- the formants (vocal_tract_freq_response) determine speech identity more strongly.

- frequency -(IDFT)-> quefrency naturally separates glottal from vocal tract.
  - lower quefrency carry information about spectral envelop
  - higher quefrency (fast changing info) carry information about spectral details

- liftering: (low pass)
  - removes all high quefrencies (removes glottal pulse)

ref: https://www.youtube.com/watch?v=4_SH2nfbQZ8&t=1191s

# Computing MFCCs

1. waveform
2. DFT
3. Log-Amp Spectrum
4. Mel-Scaling (mel filterbanks)
5. Discrete cosine transform (IDFT not used in this case)
  - DCT > IDFT
    - Discrete gives back real-value coefficients (as opposed to complex numbers)
    - De-correlate energy between distinct mel-bands
    - Reduce dimensions needed to represent spectrum
6. Profit.

- MFCC index relates to the freq of cosine applied to log spectrum.
- Higher indexes relate to higher freq cosines.

# More about MFCCs

MFCCs provide information about the phonemes. Cutting out the spectral details reduces the amount of info needed to be processed.
This makes them not a good choice for vocal synthesis.

<i> Note, traditionally MFCCs go to 12-13 index values </i>

There are 39 features of MFCC:
 - 12 MFCC features
  - 1 (log) frame energy

 - 12 Delta MFCC features (first derivative)
  - 1 Delta (log) frame energy

 - 12 Delta Delta MFCC features (second derivative)
  - 1 Delta Delta (log) frame energy

Excerpt from... https://www.sciencedirect.com/bookseries/advances-in-computers
"""
Advances in Computers: Improving the Web
Dalibor Mitrović, ... Christian Breiteneder, in Advances in Computers, 2010

5.5.1 Perceptual Filter Bank-Based Features
Bogert et al. [26] define the cepstrum as the FT of the logarithm (log) of the magnitude (mag) of the spectrum of the original signal.

This sequence is the basis for the cepstral features described in this section. However, in practice the computation slightly differs from this definition. For example, the second Fourier transform is often replaced by a DCT due to its ability to decorrelate output data.

Mel-frequency cepstral coefficients. MFCCs originate from ASR but evolved into one of the standard techniques in most domains of audio retrieval. They represent timbral information (the spectral envelope) of a signal. MFCCs have been successfully applied to timbre measurements by Terasawa et al. [13].

Computation of MFCCs includes a conversion of the Fourier coefficients to Mel-scale [34]. After conversion, the obtained vectors are logarithmized, and decorrelated by DCT to remove redundant information.

The components of MFCCs are the first few DCT coefficients that describe the coarse spectral shape. The first DCT coefficient represents the average power in the spectrum. The second coefficient approximates the broad shape of the spectrum and is related to the spectral centroid. The higher-order coefficients represent finer spectral details (e.g., pitch).

# Additional note on MFCC indexes...

<hr>
 In practice, the first 8–13 MFCC coefficients are used to represent the shape of the spectrum. However, some applications require more higher-order coefficients to capture pitch and tone information. For example, in Chinese speech recognition up to 20 cepstral coefficients may be beneficial [130]. </span>
 <br><br>
 <b> languages requiring higher order MFCCs have more emphasis on the glottal pulse as a means to communicate information.</b>
<hr>

Variations of MFCCs. In the course of time several variations of MFCCs have been proposed. They mainly differ in the applied psychoacoustic scale. Instead of the Mel-scale, variations employ the Bark- [32], ERB- [33], and octave-scale [131]. A typical variation of MFCCs are Bark-frequency cepstral coefficients (BFCCs). However, cepstral coefficients based on the Mel-scale are the most popular variant used today, even if there is no theoretical reason that the Mel-scale is superior to the other scales.

Extensions of MFCCs. A noise-robust extension of MFCCs are autocorrelation MFCCs proposed by Shannon and Paliwal [44]. The main difference is the computation of an unbiased autocorrelation from the raw signal. Particular autocorrelation coefficients are removed to filter noise. From this representation more noise-robust MFCCs are extracted.

Yuo et al. [45] introduce two noise-robust extensions of MFCCs, namely RAS-MFCCs and CHNRAS-MFCCs. The features introduce a preprocessing step to the standard computation of MFCCs that filters additive and convolutional noise (cannel distortions) by cepstral mean subtraction.

Another extension of MFCCs is introduced in Ref. [132]. Here, the outputs of the Mel-filters are weighted according to the amount of estimated noise in the bands. The feature improves accuracy of ASR in noisy environments.

Li et al. [133] propose a novel feature that may be regarded as an extension of BFCCs. The feature incorporates additional filters that model the transfer function of the cochlea. This enhances the ability to simulate the human auditory system and improves performance in noisy environment
"""


# References

1. https://musicinformationretrieval.com/about.html
2. https://jonathan-hui.medium.com/speech-recognition-feature-extraction-mfcc-plp-5455f5a69dd9
3. https://www.sciencedirect.com/bookseries/advances-in-computers
4. https://www.sciencedirect.com/topics/computer-science/cepstral-coefficient#:~:text=In%20practice%2C%20the%20first%208,Variations%20of%20MFCCs
5. https://www.youtube.com/watch?v=4_SH2nfbQZ8&t=1191s
