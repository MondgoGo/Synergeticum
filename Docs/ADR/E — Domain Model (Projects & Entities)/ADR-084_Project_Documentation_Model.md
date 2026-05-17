
## ADR-084 — Project Documentation Model

### Context & Problem
The project documentation model provides the necessary structure and guidelines to manage project documentation that is associated with each project node. The document will consist of three layers: data layer, structured layer, and AI layer. Each of these layers will have specific functionalities, and the model will ensure that the project documentation can be partially editable while preserving the integrity of the data.

### Core Principles

1. **Project Documentation as a Projection over Node**:
   - The project documentation should be considered as a view of the project's Node. This means it’s a reflection of the project's state but can be adapted to fit specific needs (such as adding explanations, user feedback, or project goals).
   - The documentation model does not modify the project’s Node data but acts as a projection that presents its information in a user-friendly way.

2. **Three Layers of Project Documentation**:
   - **Data Layer**: This contains raw project data that is essential for understanding the state of the project. It includes the metadata and core information that define the project's existence, such as timestamps, owner, and status.
   - **Structured Layer**: This layer is where all the relevant documents, discussions, and structured content related to the project are stored. It includes things like reports, analysis, progress updates, meeting notes, and other structured files that define the project’s goals and execution strategy.
   - **AI Layer**: This contains AI-generated content or summaries that enhance the understanding of the project. It includes AI-projected ideas, risks, and opportunities based on the project’s historical data and interactions, but it should never be considered the final truth source.

3. **Partial Editability**:
   - Documentation must be partially editable, meaning that while the data layer is generally immutable, the structured layer and AI projections can be updated based on ongoing discussions, outcomes, and feedback.
   - Users should have the ability to edit or add comments in the structured layer and AI-generated section, but the raw data layer remains static to maintain consistency.

4. **AI Not as the Source of Truth**:
   - The AI layer may provide suggestions, analysis, and insights, but **it is never considered the source of truth** for the project documentation. Human judgment and the data layer are always the authoritative sources.
   - AI content should be marked clearly as AI-generated and always be subject to review and approval by the relevant stakeholders.

### Decisions
1. **The Documentation Model Should Be a Projection**: 
   The documentation is an abstract layer that projects and organizes the project data in a human-readable form. It will evolve as the project moves through its lifecycle but will not alter the original project node’s state.

2. **Three-Layer Structure**:
   The data layer (immutable), the structured layer (modifiable), and the AI layer (suggestions and projections) will define the boundaries of what users can edit and interact with.

3. **Editable Structured and AI Layers**:
   Both layers are editable with constraints to ensure consistency. The structured layer will accept inputs from users and be updated over time, whereas the AI layer will evolve as the AI improves, but it is never to be relied upon as the truth source.

4. **AI’s Role**:
   The AI will provide valuable insights based on data but will always be supplementary to the human decision-making process. It should never override human judgment.

### Consequences
1. **Consistency of Data**: 
   Since the data layer is immutable, it ensures that the core project details and critical project data are consistent and unaltered.

2. **Flexibility of Structured & AI Layers**: 
   The editability of these layers allows for flexibility in updating documentation, improving the accuracy and comprehensiveness of the project’s history and insights.

3. **Stakeholder Engagement**:
   With the ability to edit and update the structured and AI layers, the stakeholders can contribute their feedback, ensuring that the documentation reflects the ongoing changes in the project while maintaining consistency.

4. **AI-Generated Content Review**:
   Since the AI layer provides projections, it will need periodic review to ensure that AI-generated content aligns with the project’s objectives and is not taken as an authoritative source without human validation.
