
# Reflection

## BrewMetrics BI Mini Project Reflection

Working on the BrewMetrics BI project with GitHub Copilot was useful for understanding and developing DAX measures. Copilot provided a good starting point for formulas such as Month-over-Month Growth, Running Total, RANKX-based City Ranking, and Average Order Value. It helped me understand which DAX functions could be used for different analytical requirements.

However, I learned that AI-generated formulas should not be copied without checking them against the actual Power BI data model. Some suggestions required correction, especially when filter context was involved. For example, the running-total calculation needed to be adjusted using `ALLSELECTED()` so that report selections were handled appropriately. The city ranking measure also required `ALL()` so that cities could be compared against the complete set of cities.

Using GitHub changed the way I approached the project compared with a normal single-file Power BI lab. Instead of building everything at once, I developed the solution incrementally. I committed the star schema first, followed by individual DAX measures and then the completed dashboard. This made each stage traceable and allowed me to see how the project evolved.

Overall, the combination of Power BI, GitHub, and Copilot made the project more structured, reviewable, and closer to a real-world BI development workflow.
