---
layout: post
title: "What is deformable neural network and how it works"
date: 2025-02-13
categories: tutorials
tags: [machine-learning]
permalink: /posts/deformable-neural-network/
usemathjax: true
---



## **1. How Deformable Convolution Works: Intuition**

### **The "Rubber Grid" Analogy**
Think of the convolution grid as a **rubber sheet**. For each input image, the network *stretches* or *shrinks* parts of the sheet to align with object shapes.  

**Example**:  
- **Task**: Detect a bird’s wings in flight.  
- **Challenge**: Wings might be spread wide or folded.  
- **DCN Action**: The grid "warps" to sample wingtip pixels whether they’re at (100,50) or (95,55).  

---

## **2. The Math: Offset Learning and Sampling**

### **2.1 Positional Offset**
For a standard 3x3 convolution, the kernel samples 9 positions around a central pixel. In DCNs:  
1. **Offset Layer**: A parallel convolutional layer predicts **18 values** (9 x 2 for x/y offsets) per output pixel.  
2. **Offset Application**: Adjust the 9 sampling positions using these offsets.  

**Example**:  
- Original grid positions:  
  $$ (-1,-1), (-1,0), (-1,1), \dots, (1,1) $$  
- Predicted offsets:  
  $$ (\Delta x_1, \Delta y_1), \dots, (\Delta x_9, \Delta y_9) $$  
- New sampling positions:  
  $$ (-1+\Delta x_1, -1+\Delta y_1), \dots, (1+\Delta x_9, 1+\Delta y_9) $$  

### **2.2 Bilinear Sampling in Action**
Suppose a sampling position shifts to **(5.3, 7.8)** – not an integer grid point. To compute the value here:  
1. **Identify 4 Neighbors**: (5,7), (5,8), (6,7), (6,8).  
2. **Compute Weights**:  
   - For x: 5.3 is 0.3 away from 6 → weight for column 5: **0.7**, column 6: **0.3**.  
   - For y: 7.8 is 0.8 away from 8 → weight for row 7: **0.2**, row 8: **0.8**.  
3. **Interpolate**:  
   - Top row: $$ 0.7 \times \text{Value}(5,7) + 0.3 \times \text{Value}(6,7) $$  
   - Bottom row: $$ 0.7 \times \text{Value}(5,8) + 0.3 \times \text{Value}(6,8) $$ 
   - Final value: $$ 0.2 \times \text{Top} + 0.8 \times \text{Bottom} $$x  

---

## **3. Common Pitfalls and Solutions**

### **3.1 Over-Adaptation**
- **Problem**: Offsets become too large, sampling random regions.  
- **Fix**: Regularize offset magnitudes (e.g., L2 penalty).  

### **3.2 Edge Artifacts**
- **Problem**: Offsets near image borders sample "outside" the image.  
- **Fix**: Use padding (e.g., reflection padding)