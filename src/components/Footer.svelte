<footer>
  <h3>
    Writeup:
  </h3>
  <p>
    We embarked on a project aimed at identifying the top rebounders in the current NBA, focusing on player positions. Initially, we attempted to use a bar plot to compare rebound statistics. However, we quickly realized that the varying minutes played by each player skewed the comparison. To address this, we transitioned to a scatter plot, leveraging its ability to showcase the correlation between minutes played and rebounds.
  </p>
  <p>
    As we plotted all players on a single graph, the sheer volume of data made it challenging to discern individual data points. To enhance usability, we implemented a player search bar, enabling users to find specific players of interest efficiently. Additionally, when exploring the correlation between player positions and rebounds, we encountered issues with overlapping data points. To mitigate this, we opted to display data points only when a specific position is selected, enhancing interactivity and clarity.
  </p>
  <p>
    Dividing the workload, we allocated time to data collection and model development. We both dedicated five hours to studying tutorials and examples online, equipping ourselves with the necessary knowledge. Lincoln focused on constructing the scatter plot for five hours, while Brighton spent an additional seven hours refining the visualization by adding player highlighting and position features.
  </p>
  <p>
    Despite successfully creating the visualization on our localhost, we encountered challenges when deploying it to Git. Collaboratively, we invested over 10 hours debugging the deployment issues, ensuring the functionality of the visualization on Git. Building the graph proved to be the most challenging aspect, requiring mastery of new syntax and navigating limitations in accessing certain objects. Nonetheless, the project was an enjoyable learning experience, equipping us with valuable skills for our future careers.
  </p>
  
  <p>
    Developed by Brighton Chan and Lincoln Ma
    &middot; Source:
    <a
      href="https://www.nba.com/stats/"
      target="_blank"
      rel="noopener noreferrer"
    >
      NBA
    </a>
    &middot; Data downloaded on February 18, 2024
  </p>
</footer>

<style>
  footer {
    margin-top: 5px;
    max-width: 600px;
  }

  p {
    line-height: 25px;
  }

  a {
    color: #4427ca;
    text-decoration-thickness: 0.0625rem;
    text-underline-offset: 0.1em;
  }

  a:visited {
    color: #4427ca;
  }

  a:active {
    color: #dc267f;
    text-decoration-thickness: 0.2rem;
  }

  a:hover {
    color: #dc267f;
    text-decoration-thickness: 0.2rem;
  }
</style>
