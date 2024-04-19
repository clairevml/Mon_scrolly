<script>
  import { onMount, onDestroy } from 'svelte';

import LoremIpsum from './LoremIpsum.svelte'

	import Introduction from './Introduction.svelte'

  let activeStepIndex = 0;
  let observer;
  let scrollyRef;
  let flourishID = "2342286"; // L'ID de la story Flourish

// le texte des boites
	
  let stepsData = [
    { "text": "Les petites entreprises ont encore peu recours à l'intelligence artificielle pour les aider dans les compétences écrites et parlées. <mark style='background-color: #900C3F; color:white; padding: 2px; border-radius: 5px;'><strong>En mauve</strong></mark>, on voit que <mark style='background-color: #900C3F; color:white; padding: 2px; border-radius: 5px;'><strong>3 à 6%</strong></mark> des entreprises de 10 employés et plus l'utilisaient pour cette fonction en 2023.</strong></mark>" },
    { "text": "En revanche, beaucoup plus de grosses entreprises belges ont recours à l'IA. On voit <mark style='background-color: #dcb233 ; color:white; padding: 2px; border-radius: 5px;'><strong> en jaune </strong></mark> que <mark style='background-color: #dcb233 ; color:white; padding: 2px; border-radius: 5px;'><strong>16 à 23% </strong></mark>des entreprises de 250 employés et plus les utilisent pour les aider dans les tâches écrites ou parlées." },
    { "text": "La tendance se confirme pour d'autres fonctions. Les petites entreprises utilisent moins l'IA pour l'automatisation que pour les fonctions écrites : <mark style='background-color: #900C3F; color:white; padding: 2px; border-radius: 5px;'><strong>entre 2 et 6%.</strong></mark> En revanche, <mark style='background-color: #dcb233 ; color:white; padding: 2px; border-radius: 5px;'><strong>presque 30% grosses entreprises</strong></mark> utilisent l'IA pour le flux de travail ou la prise de décision.  " },
		{ "text": "Automatiser <mark style='background-color: #008000 ; color:white; padding: 2px; border-radius: 5px;'><strong>le flux de travail</strong></mark> c'est utiliser la technologie pour simplifier les tâches et les activités dans une entreprise. Aider à la <mark style='background-color: #008000 ; color:white; padding: 2px; border-radius: 5px;'><strong>prise de décision</strong></mark> au travail c'est renforcer les capacités humaines, en analysant les données, en prévoyant les tendances, en simplifiant les processus." },
		{ "text": "<mark style='background-color: #008000 ; color:white; padding: 2px; border-radius: 5px;'><strong>L'apprentissage automatique</strong></mark> est une branche de lintelligence artificielle. Son objectif est qu'une machine soit capable de penser et dapprendre par elle-même grâce à des données et des algorithmes. Presque <mark style='background-color: #008000 ; color:white; padding: 2px; border-radius: 5px;'><strong>30% des grosses entreprises belges</strong></mark> l'utilisent." },
  ];

	// Le "moteur" du scrollytelling qui utilise l'Intersection Observer API (en gros, le code observe ce qu'il y a à l'écran)
	// ça on ne touche pas sinon tout se casse !

  onMount(() => {
    observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        const { target, isIntersecting } = entry;
        const index = Array.from(scrollyRef.querySelectorAll('.step')).indexOf(target);
        if (isIntersecting) {
          activeStepIndex = index;
        }
      });
    }, { threshold: 0.6 });

    const stepElements = scrollyRef.querySelectorAll('.step');
    stepElements.forEach(el => observer.observe(el));
  });

  onDestroy(() => {
    observer.disconnect();
  });
</script>

<h1>À quel point les entreprises belges utilisent-elles l'IA ?</h1>

<Introduction/>

<!-- si tu affiches la step_0, alors montre la slide_0 -->
<!-- si tu affiches la step_1, alors montre la slide_1 -->
<!-- si tu affiches la step_2, alors montre la slide_2 -->

<section bind:this={scrollyRef} class="section-container">

	  <div class="foreground">
    {#each stepsData as { text }, index}
      <div class="step" data-step={index}>
        <div class="step-content">
          <p>{@html text}</p> <!-- @html important pour que le css du script soit pris en compte-->
        </div>
      </div>
    {/each}
  </div>
	
  <div class="sticky-background">
    <!-- On affiche la slide Flourish en fonction de l'index -->
		<iframe src={`https://flo.uri.sh/story/${flourishID}/embed#slide-${activeStepIndex}`} title="Contenu interactif ou visuel" class="flourish-embed" frameborder="0" scrolling="no" style="width:100%;height:100vh;"></iframe>
  </div>


</section>


<style>

	/* Ici les valeurs pour l'ensemble de la page > 
	peut nécessiter des modifs de couleurs dans Flourish 
	pour s'assurer que le graphe soit tjs bien visible (titre de graphique noir sur
	fond de page noir,ça ne se voit pas bien...*/

	:global(body) {
    background-color: #312c60; 
		color: white;
		font-family: 'Georgia', sans-serif; 
  }
	
	*{
		box-sizing: border-box;
	}
	.section-container {
    margin-top: 1em;
    text-align: center;
    display: flex;
    flex-direction: row; 
		
  }

  .sticky-background {
    position: sticky;
    top: 0;
    height: 100vh;
    flex: 0 1 60%;
    overflow: hidden;
    z-index: 1;
  }

  .foreground {
    display: flex;
    flex-direction: column;
    align-items: center;
    flex: 0 1 40%;
		margin-bottom: 100vh;
  }

  .step {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
    position: relative;
    z-index: 2;
    width: 100%;
  }

  .step-content {
    background-color: rgba(289, 289, 289, 1.5);
		box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2);
    color: black;
    border-radius: 10px;
		border:2px solid #01257D;
    padding: 0.5rem 1rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    z-index: 10;
    font-size: 1rem;
    text-align: center;
    width: 100%; 
    max-width: 500px; 
    margin: auto;
  }

	/* Pour adapter la vue en mobile: steps centrées par dessus le graphique */

  @media screen and (max-width: 768px) {
    .section-container {
      flex-direction: column-reverse;
    }
    .sticky-background, .foreground {
      width: 100%; 
    }
    .foreground {
      margin-top: -80vh; 
    }
  }
</style>

