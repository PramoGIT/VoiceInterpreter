# VoiceInterpreter

# Voice Interpreter: Testing and Performance Evaluation

## 1. Introduction

The purpose of this project was to develop a Python voice interpreter that converts spoken English into spoken French. The application uses speech-to-text (STT) to recognise English speech, a translation model to translate the recognised text into French, and text-to-speech (TTS) to generate French audio.

The application was developed and tested using Python in Visual Studio Code.

## 2. Testing Methodology

Five test runs were conducted. For each test, an English sentence was spoken into the microphone for approximately five seconds. The application recognised the speech, translated the transcript into French, generated an audio file, and played the resulting French speech.

The measured processing stages were speech recognition time, translation time, TTS generation time, and total delay from the end of recording to the start of audio playback.

## 3. Test Results

| Test | STT (s) | Translation (s) | TTS (s) | Total delay (s) |
|---|---:|---:|---:|---:|
| 1 | 6.00 | 12.78 | 29.17 | 48.02 |
| 2 | 2.66 | 17.32 | 38.57 | 58.72 |
| 3 | 4.50 | 9.88 | 14.66 | 29.07 |
| 4 | 4.20 | 8.31 | 18.59 | 31.13 |
| 5 | 4.46 | 7.50 | 17.39 | 29.37 |

## 4. Performance Analysis

The fastest total delay was 29.07 seconds in Test 3. The median delay was 31.13 seconds, and the slowest delay was 58.72 seconds in Test 2. The average total delay was approximately 39.26 seconds.

The average STT time was 4.36 seconds, the average translation time was 11.16 seconds, and the average TTS generation time was 23.68 seconds.

The results show that text-to-speech generation was the main performance bottleneck. Translation was the second-slowest stage, while speech recognition was the fastest stage on average.

Processing times varied between tests. Possible reasons include sentence length, model initialisation, and available computer resources.

## 5. Limitations and Possible Improvements

The current implementation generates the complete French audio file before playback. Consequently, the reported TTS first-playable-audio time is the full audio-generation time rather than a true streaming time-to-first-audio measurement.

Performance could be improved by keeping models loaded between requests, using smaller or faster models where appropriate, reducing unnecessary file operations, and implementing streaming TTS so playback can begin before the complete audio file is generated.

The English speech recognition occasionally omitted the end of a sentence. Recording duration and end-of-speech detection could be improved to capture complete sentences more reliably.


### 1. Where is most of the delay produced?
 Most of the delay is produced during Text-to-Speech (TTS), which converts the translated French text into spoken audio.
Based on my five test runs, the average processing times were:
Processing stage	Average time
Speech-to-Text (STT)	4.36 seconds
Translation	11.16 seconds
Text-to-Speech (TTS)	23.68 seconds
TTS is the main bottleneck because it takes the longest time to generate the French audio. The average total delay from the end of recording until playback begins was approximately 39.26 seconds.

### 2. What could I change to reduce the delay?
 I could make several improvements:
1. Use a faster TTS model: Choose a smaller or faster voice model to reduce audio-generation time.
2. Keep models loaded in memory: Avoid reloading the speech recognition and translation models every time the application runs.
3. Optimise translation: Use a faster translation model and avoid unnecessary processing.
4. Reduce file operations: Avoid saving and reloading complete audio files when they are not necessary.
5. Use streaming: Generate and play audio in smaller chunks instead of waiting for the entire French audio file to be created.
3. Extra challenge: Implement streaming
Currently, the application follows this process:
English speech recording


Speech recognition and translation


Generate the complete French audio file


Start audio playback


With streaming, the application could begin playing the first French audio chunk while later chunks are still being generated.
English speech recording


Recognise speech and translate text


Play chunk 1


Generate chunk 2


Generate chunk 3


Play next chunks



Audio generation and playback can overlap, reducing the time the user waits before hearing French speech.

To implement this, I could split the translated French text into sentences or smaller chunks, generate audio for each chunk, and play each chunk as soon as it is ready. A more advanced implementation would use a TTS engine that supports streaming audio output directly.
Conclusion: TTS generation is the main bottleneck in my current application. Using a faster model and implementing streaming could reduce the waiting time and make the interpreter more responsive. Streaming may reduce the time before playback begins, although the total time to finish speaking all the audio may not decrease by the same amount.

## 6. Conclusion

The Voice Interpreter successfully demonstrated the complete English-speech-to-French-speech workflow in all five test runs. The timing measurements identified TTS generation as the main bottleneck and provided a baseline for future optimisation. Further improvements to audio generation, recording accuracy, and streaming could reduce the delay and improve the user experience.
