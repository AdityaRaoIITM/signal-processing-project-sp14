# Audio Enhancer with Automatic Signal Optimization

**SP14 - Signal Processing Project**  
**Aditya Rao - 23F3000019**

## Overview

This is a web-based audio enhancement application that provides real-time audio processing with advanced automatic signal optimization. The application addresses the goal of optimizing signal output by automatically amplifying quiet audio (below -10 dBFS) to a comfortable listening level, effectively providing up to 50dB of gain for very quiet content.

## Features

### Signal Processing
- **Master Gain Control**: Adjust overall audio volume (0-3x)
- **Pitch Factor**: Change playback speed/pitch (0.5-2.0x)
- **Bass Equalization**: Boost or cut low frequencies (-10 to +15dB)
- **Treble Equalization**: Boost or cut high frequencies (-10 to +15dB)
- **Echo Effect**: Add echo with adjustable intensity (0-0.8)

### Audio Boost Optimization (New)

The application now includes an automatic signal optimization feature that ensures quiet audio is amplified to a comfortable listening level.

#### How It Works

1. **Real-time Level Monitoring**: An AnalyserNode continuously measures the audio signal's RMS (Root Mean Square) level and converts it to decibels Full Scale (dBFS).

2. **Dynamic Gain Adjustment**: When audio levels fall below the target (-10 dBFS), the system automatically calculates the required gain boost using the formula:
   ```
   gainAdjustment = 10^((targetDb - currentDb) / 20)
   ```
   The target of -10 dBFS provides loud, clear audio with 10dB headroom to prevent clipping.

3. **Smooth Amplification**: The gain is applied gradually with a smoothing factor of 0.1 to prevent sudden volume jumps, ensuring a pleasant listening experience.

4. **Clipping Prevention**: 
   - Maximum gain is capped at 10x (20dB boost) to prevent extreme amplification
   - A DynamicsCompressor node provides additional protection against clipping
   - Compressor settings: threshold=-24dB, ratio=12:1, attack=3ms, release=250ms

5. **Visual Feedback**: The current audio level is displayed with color coding:
   - 🔴 Red (< -40 dBFS): Very quiet, needs significant boost
   - 🟡 Yellow (-40 to -20 dBFS): Quiet, moderate boost applied
   - 🟢 Green (≥ -20 dBFS): Good level achieved

#### Technical Implementation

**Audio Processing Chain** (when optimization is enabled):
```
Source → Bass Filter → Treble Filter → Master Gain → 
Analyser → Compressor → Optimization Gain → Output
```

**Key Parameters**:
- Target Level: -10 dBFS (loud and clear with headroom)
- Monitoring Interval: 100ms
- Smoothing Factor: 0.1 (10% adjustment per interval)
- Maximum Gain: 10x (20dB boost limit)
- dBFS Scale: 0 dBFS = maximum digital level (1.0 RMS)

#### Usage

1. Upload an audio file using the "Select Audio File" button
2. The Audio Boost is enabled by default (toggle to disable if needed)
3. Press play and watch the "Current Level" indicator
4. Audio below -10 dBFS will be automatically amplified to that level
5. The indicator color shows the optimization status in real-time

**Example Results**:
- Quiet audio at -30 dBFS → Amplified by 10x → Reaches -10 dBFS ✓
- Very quiet audio at -40 dBFS → Amplified by 10x (capped) → Reaches -20 dBFS ✓
- Moderate audio at -20 dBFS → Amplified by 3.16x → Reaches -10 dBFS ✓
- Already loud audio at -6 dBFS → No amplification needed ✓

### Voice Presets

- **Male Voice**: Lower pitch (0.85x) with bass boost (+6dB) and treble cut (-2dB)
- **Female Voice**: Higher pitch (1.4x) with treble boost (+5dB) and bass cut (-2dB)
- **Raw**: Original audio without processing

## Technical Details

- **Technology**: Web Audio API, HTML5, JavaScript
- **Audio Nodes Used**: 
  - BufferSourceNode (audio playback)
  - BiquadFilterNode (bass/treble EQ)
  - GainNode (volume control)
  - DelayNode + Feedback (echo effect)
  - AnalyserNode (level measurement)
  - DynamicsCompressorNode (dynamic range control)
- **Framework**: Tailwind CSS for styling
- **Icons**: Lucide Icons

## Browser Compatibility

Requires a modern browser with Web Audio API support:
- Chrome 34+
- Firefox 25+
- Safari 14.1+
- Edge 79+

## License

Educational project for IIT Madras Signal Processing course (SP14)

## Contact

Aditya Rao - 23f3000019@es.study.iitm.ac.in
