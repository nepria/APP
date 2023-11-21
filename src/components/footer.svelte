<script lang="ts">

import { onMount } from "svelte";
import { letterSlideIn, maskSlideIn } from "../animations";
import { dataFetch } from "../store"

let footerContainer;
let logoElem, creditsElem, statusElem, fullEmailLinkElem;
let signaturePath1, signaturePath2, signaturePath3, signaturePath4; 

let currentYear = new Date().getFullYear();

onMount(() => {
    introAnimations();
});

async function introAnimations() {
    // Scroll activated animations powered by anime instead of svelte transitions
	const logoAnimate = maskSlideIn(logoElem, {});
    const fullEmailLinkAnimate = letterSlideIn(fullEmailLinkElem, { delay: 6, initDelay: 150 });
    const creditsAnimate = maskSlideIn(creditsElem, { delay: 150 });

    // Intersection observer to run animations when footer is in scroll view
    let animationObserver = new IntersectionObserver((entries) => { 
        entries.forEach(entry => {
            if (entry.isIntersecting) {

				logoAnimate.anime();
                creditsAnimate.anime();
                statusAnimate.anime();
				fullEmailLinkAnimate.anime();

                // Signature SVG animation
                let animation = [{ strokeDashoffset: '0' }];

                // Signature animation using svg strokDashOffset
                signaturePath1.animate(animation, {
                    duration: 1000,
                    delay: 0,
                    easing: 'cubic-bezier(.72,.3,.25,1)',
                    fill: 'forwards' 
                });
                signaturePath2.animate(animation, {
                    duration: 300,
                    delay: 1000,
                    easing: 'cubic-bezier(.47,.41,.26,1)',
                    fill: 'forwards' 
                });
                signaturePath3.animate(animation, {
                    duration: 200,
                    delay: 1300,
                    easing: 'cubic-bezier(.47,.41,.26,1)',
                    fill: 'forwards' 
                });
                signaturePath4.animate(animation, {
                    duration: 1000,
                    delay: 1500,
                    easing: 'cubic-bezier(.47,.41,.26,1)',
                    fill: 'forwards' 
                });

                animationObserver.disconnect();
            }
        });
    }, {
        root: null,
        threshold: 0.4
    });

    animationObserver.observe(footerContainer);

    await dataFetch;
	const statusAnimate = letterSlideIn(statusElem, { delay: 6, initDelay: 100 });
}

</script>



<div class="footer-wrapper" bind:this={footerContainer}>
    <!-- Left side -->
    <div class="flex-wrapper">
        <div class="logo-wrapper">
            <div class="inline-flex" bind:this={logoElem}>
                <img src="assets/imgs/logo.png" alt="app logo" class="logo">
            </div>
        </div>

        <div class="status-wrapper">
            {#await dataFetch then fetchedData}
                {#if fetchedData.availablity_date == ""}
                    <p class="large-text" bind:this={statusElem}>
                        i am currently accepting freelance work, <br>you may reach me on my email.
                    </p>
                {:else}
                    <p class="large-text" bind:this={statusElem}>
                        i am available for freelance work after <br> {fetchedData.availablity_date}.
                    </p>
                {/if}
            {/await}
            <a class="button large-text" bind:this={fullEmailLinkElem} href="mailto:app@gmail.com" target="_blank">app@gmail.com</a>
        </div>
        
        <div class="credits-wrapper" bind:this={creditsElem}>
            <p class="year">© {currentYear}</p>
            <p class="credits">
                designed and developed by akhouri priyasha<br>this webste is open source on github
            </p>
        </div>
    </div>

    <!-- Right side -->
	<div class="flex-wrapper decor">
       <img src="assets/imgs/signature-white.png" alt="app logo" class="logo" style="padding-left: 450px; width:50%; height: 100%; ">
    </div>
</div>



<style lang="sass">

@import "../consts.sass"
@include textStyles()

.footer-wrapper
    width: 100vw
    background-color: #131314
    display: flex
    flex-direction: row
    justify-content: space-between
    padding: 15vh 13vw
    margin-top: 25vh
    box-sizing: border-box

    @media only screen and (max-width: 950px)
        .flex-wrapper.decor
            display: none !important

    @media only screen and (max-width: 950px)
        flex-direction: column-reverse

        .flex-wrapper:not(:first-child)
            margin-bottom: 15vh

    .inline-flex
        flex-grow: 1
        display: flex
        flex-direction: row
        align-items: center


    .logo-wrapper
        margin-bottom: 5vh

        .logo
            display: inline-block
            height: 6vh

    .status-wrapper
        .button.large-text
            margin-top: 2vh

    .credits-wrapper
        margin-top: 5vh
        color: rgba(255,255,255,0.3)

        p.year
            margin-bottom: 1vh
            font-family: $font
            font-size: 1.8vh
            font-weight: normal
            color: rgba(255,255,255,0.3)

        .credits
            font-size: 1.5vh
            line-height: 125%
            white-space: nowrap
            color: rgba(255,255,255,0.3)

    .large-text
        font-size: 2.5vh

        @media only screen and (max-width: 950px)
            br
                display: none

    .flex-wrapper.decor
        display: flex
        flex-direction: column
        justify-content: center

        .name-signature
            width: 20vh

#signature
    .path-1
        stroke-dasharray: 365
        stroke-dashoffset: 365
    
    .path-2
        stroke-dasharray: 85
        stroke-dashoffset: 85

    .path-3
        stroke-dasharray: 45
        stroke-dashoffset: 45

    .path-4
        stroke-dasharray: 180
        stroke-dashoffset: 180

</style>