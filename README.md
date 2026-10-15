# Interactive Project Kiosk Application

A prototype touch-kiosk interface specification and responsive dashboard prototype designed for collaborative agile project creation, cost roadmap estimation, and team authorization.

## Overview

Designed for physical meeting rooms and collaborative hub displays, the Kiosk Application provides intuitive touch-first UI workflows for creating project initiatives, estimating delivery phases, and viewing interactive budget roadmaps.

## Project Structure

```
KioskApp/
├── docs/
│   ├── AuthContext.md         # Authorization context, user roles, and access boundaries
│   ├── CostRoadmaps.md        # Cost forecasting models and roadmap timelines
│   ├── ProjectCreation.md     # Project intake wizard specifications
│   ├── Projects.md            # Active project status specifications
│   └── UpdateProject.md       # Mutation workflows and state transitions
├── index.html                 # Interactive kiosk UI prototype
├── .gitignore                 # OS ignore rules
├── LICENSE                    # MIT License
└── README.md                  # Project overview and specifications guide
```

## Running the Prototype

Open `index.html` in any browser or launch a local web server:
```bash
npx serve .
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
