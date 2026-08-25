# Statswatch — Overwatch Gameplay Analytics

> 🚧 **Work in Progress**

Statswatch is a gameplay analytics platform designed for competitive Overwatch players and teams. The platform combines existing gameplay telemetry with **computer vision** and **machine learning** to create a richer, more granular view of match performance.

Unlike traditional Overwatch analytics, Statswatch is designed to analyze gameplay not only at the match and map level, but also at the **objective and individual team-fight level**.

---

## Overview

Overwatch analytics have historically been limited by the data available from the game itself.

Early analysis primarily relied on aggregate profile and match statistics provided through Blizzard's APIs. These datasets can answer broad questions about player and match performance, but provide limited context about what actually happened during a game.

More advanced analysis tools use **Overwolf's Game Events Provider (GEP)** to access substantially more granular gameplay telemetry, including information such as:

* Hero selections
* Kills and deaths
* Damage and healing
* Map information
* Timestamps
* Other gameplay events

However, even this richer telemetry leaves important gaps. Information that can be valuable for competitive analysis—including:

* Ability usage
* Ultimate usage
* Player positions
* Objective progress
* Perks
* Other visual game-state information

is not fully available through GEP.

Statswatch attempts to close some of these gaps by combining telemetry with information extracted directly from gameplay footage.

---

# What Makes Statswatch Different?

Statswatch focuses on two major capabilities that expand the granularity of Overwatch analysis.

## 1. Computer Vision-Based Data Extraction

Instead of relying solely on available telemetry, Statswatch uses **computer vision applied directly to gameplay footage** to recover information that existing data sources do not provide.

Current computer-vision systems extract information such as:

* Payload progress
* Capture progress
* Objective timers
* Score
* Robot type for Push gamemode
* Perk information

This creates a richer representation of a match than either gameplay telemetry or video analysis can provide independently.

The dataset is still incomplete, and there are gameplay elements we have not yet been able to capture. Expanding this coverage is an ongoing part of development.

### Computer Vision Example

**[SCREENSHOT PLACEHOLDER — Insert annotated gameplay screenshot showing CV detections, e.g. payload progress, timer, score, etc.]**

*Example of objective and game-state information being extracted directly from gameplay footage.*

---

## 2. Team-Fight Segmentation

Having more data is only part of the problem. Statswatch also introduces a new way to analyze that data by using **machine learning to segment matches into individual team fights**.

Instead of treating an entire match or map as the primary unit of analysis, Statswatch can break gameplay down into individual engagements:

**Match → Map → Objective → Team Fight → Events**

This enables analysis that traditional match-level statistics cannot provide.

For example:

* Who was picked first in a team fight?
* Which heroes were already dead when a fight began?
* What was the objective state when the fight occurred?
* Which players survived successful fights?
* How often does a team win after losing the first player?
* Which gameplay patterns correlate with successful fights?

The current team-fight segmentation system is an early implementation using a **Gaussian mixture model** and is still being developed.

### Team-Fight Segmentation Example

**[SCREENSHOT PLACEHOLDER — Insert screenshot/visualization showing match timeline divided into team fights]**

*Early visualization of the team-fight segmentation process.*

---

# Making Granular Analysis Usable

More data and more granular analysis create another challenge:

> **How do you make all of this information useful without overwhelming the user?**

Statswatch' UX/UI is designed to bridge that gap.

With analysis spanning matches, maps, objectives, team fights, and individual events, users need an intuitive high-level view while still having the ability to progressively **drill down into the details**.

The goal is to create a path from:

**"How did I perform?"**
↓
**"Where did I struggle?"**
↓
**"What happened at that objective?"**
↓
**"What happened during that team fight?"**
↓
**"What specifically led to the outcome?"**

The UX/UI brings the data, computer vision, and machine-learning components together into a single experience, giving competitive players and teams both **clarity at a glance and the ability to dive deep when needed**.

### Current UX/UI

**[SCREENSHOT PLACEHOLDER — Insert best dashboard screenshot]**

*Current dashboard design. The interface is actively evolving alongside the underlying data and analytics capabilities.*

**[SCREENSHOT PLACEHOLDER — Insert second dashboard/page screenshot]**

*Additional view showing current analytics or navigation design.*

---

# How It Works

At a high level, Statswatch combines multiple data sources and processing stages:

```text
                    Gameplay Video
                          │
                          ▼
                 Computer Vision
                          │
                  Objective Data
                          │
                          ▼
Gameplay Telemetry ──► Unified Dataset
                          │
                          ▼
                Team-Fight Segmentation
                          │
                          ▼
                  Analytics Layer
                          │
                          ▼
                    User Interface
```

## 1. Gameplay Telemetry

Overwolf GEP provides structured gameplay information including:

* Hero selections
* Kills and deaths
* Damage and healing
* Map information
* Timestamps
* Other gameplay events

This provides the foundation for the platform's gameplay dataset.

---

## 2. Computer Vision

Gameplay footage is analyzed to recover information that is unavailable through existing telemetry.

Different visual signals require different approaches.

### Template Matching

Used to identify visual elements such as:

* Score
* Robot type
* Other consistent game-state elements

### OCR

Used to extract text-based information such as:

* Objective timers

### Color / HUE Analysis

Used to interpret visual progress indicators such as:

* Payload progress
* Capture progress
* Other color-based objective states

The current system has been evaluated using manually labeled gameplay frames. Individual objective-related detection tasks currently achieve approximately **95–98% accuracy**, with performance varying by the type of information being extracted.

---

## 3. Data Integration

The next stage of development is integrating computer-vision outputs with the existing gameplay telemetry.

The goal is to associate CV-derived information with the existing event data using timestamps, creating a unified dataset that contains both:

> **What the telemetry tells us happened**

and

> **What can be observed directly from the gameplay footage**

This combined dataset will provide the foundation for the next stages of analysis.

---

## 4. Team-Fight Segmentation

The combined gameplay dataset can then be used to identify the beginning and end of individual team fights.

The current implementation uses a **Gaussian mixture model** as an initial approach to identifying team-fight segments from the available gameplay signals.

This component is still in an early stage and will evolve as additional data becomes available and the segmentation approach is refined.

**[SCREENSHOT PLACEHOLDER — Insert team-fight timeline/model output]**

---

## Analytics & UX

The richness of the underlying dataset creates an important product and UX challenge: **how should competitive players and teams actually interact with this level of information?**

Statswatch is designed to support two complementary approaches to analysis.

### Longitudinal Analysis

By aggregating data across multiple matches, users can identify patterns in their gameplay over time.

Rather than focusing on a single game, players could explore questions such as:

* Which heroes or compositions consistently perform well?
* Where do performance issues occur most frequently?
* How often does a particular team-fight pattern lead to a loss?
* How does performance change across different maps and objectives?

This provides a higher-level view of **recurring strengths, weaknesses, and trends**.

### Granular Match Analysis

Longitudinal patterns are only useful if users can understand what caused them.

Statswatch therefore also supports progressively drilling into the underlying gameplay:

**Longitudinal Pattern → Match → Map → Objective → Team Fight → Events**

This allows users to move from a broad observation to the specific situations that contributed to it.

For example:

> **"We consistently lose fights on King's Row Point A."**
> ↓
> **"Which matches contributed to this pattern?"**
> ↓
> **"What happened during those fights?"**
> ↓
> **"Who was picked first?"**
> ↓
> **"What was the objective state?"**

### An Evolving UX Direction

One of the ongoing product questions is **how much emphasis should be placed on longitudinal analysis versus individual-match analysis**.

The platform is therefore being designed with both perspectives in mind while we explore which workflows provide the most value to competitive players and teams.

The goal is not simply to expose every available statistic. It is to create a clear path between **high-level patterns and the granular gameplay events behind them**, while giving users the ability to decide how deeply they want to investigate.

---

# Project Status

> 🚧 **Active Development**

Statswatch is a collaborative project and is still under development. The architecture, data pipeline, machine-learning models, and UX/UI are continuing to evolve as we expand the available data and refine how it is analyzed and presented.

The screenshots and functionality shown in this repository represent the **current development state, not the final product**.

### Current Progress

* [x] Gameplay telemetry integration through Overwolf GEP
* [x] Identify key gaps in existing gameplay telemetry
* [x] Initial computer-vision objective detection
* [x] Computer-vision evaluation
* [x] Initial UX/UI and dashboard designs
* [ ] Integrate computer-vision outputs with gameplay telemetry
* [ ] Build unified data pipeline
* [ ] Refine team-fight segmentation model
* [ ] Develop team-fight-level analytics
* [ ] Expand objective-level analytics
* [ ] Expand computer-vision coverage
* [ ] Complete production dashboard and user experience
* [ ] End-to-end testing and validation

---

# My Contributions

Statswatch is a **collaborative project**, with contributors working across computer vision, machine learning, data engineering, analytics, and product development.

My work so far has focused primarily on **computer vision and UX/UI**, with additional data engineering and machine-learning contributions planned as development progresses.

## Computer Vision

I have worked on the computer-vision component responsible for recovering gameplay information that is not available through existing telemetry.

My contributions include:

* Identifying gameplay information that would provide additional analytical value
* Determining how the required information could be extracted from gameplay footage
* Designing and developing OpenCV-based detection approaches
* Implementing template matching for visual game-state elements
* Implementing OCR for objective timers
* Developing HUE/color-based analysis for visual progress indicators
* Creating and labeling evaluation data
* Tuning detection parameters
* Evaluating detection performance

Current objective-related detection approaches achieve approximately **95–98% accuracy across individual tasks**, with performance varying depending on the information being extracted.

---

## UX/UI

I am also responsible for designing the user experience and interface for the analytics platform.

My work includes:

* Information architecture
* Dashboard design
* Data visualization
* Navigation and interaction design
* Designing ways to expose granular objective- and team-fight-level information without overwhelming users
* Creating a progression from high-level performance summaries to detailed gameplay analysis

The goal is to make the additional complexity created by the richer dataset useful rather than overwhelming.

---

## Upcoming Contributions

As development continues, I will also be working on:

* Integrating computer-vision outputs with gameplay telemetry
* Developing data pipelines for the combined dataset
* Contributing to the team-fight segmentation model
* Expanding team-fight-level analytics
* Continuing to refine the UX around new analytical capabilities

---

# Screenshots & Development Progress

The following screenshots show the current state of the project. Because Statswatch is actively being developed, the interface and analytical outputs are expected to change significantly over time.

## Dashboard

**[SCREENSHOT PLACEHOLDER]**

*Current dashboard / primary analytics view.*

## Objective-Level Analysis

**[SCREENSHOT PLACEHOLDER]**

*Current concept for analyzing performance by map objective and game state.*

## Team-Fight Analysis

**[SCREENSHOT PLACEHOLDER]**

*Early visualization of team-fight segmentation and fight-level statistics.*

## Computer Vision

**[SCREENSHOT PLACEHOLDER]**

*Example of objective and game-state information extracted from gameplay footage.*

---

# Future Direction

The long-term goal of Statswatch is to move Overwatch analysis beyond traditional match-level statistics.

Rather than asking only:

> **"What happened in this match?"**

Statswatch aims to make it possible to ask:

> **"What happened at this objective, during this team fight, and what contributed to the outcome?"**

Future development will focus on expanding the available gameplay data, improving team-fight segmentation, building deeper analytics, and refining the UX that brings everything together.

Ultimately, the goal is to turn raw gameplay footage and telemetry into a structured dataset capable of supporting **deep, event-level competitive analysis** while keeping those insights accessible to the people who use them.
