<script lang="ts">
    import ProjectCard from '$lib/components/ProjectCard.svelte';
    import { onMount } from 'svelte';
    import emailjs from '@emailjs/browser';

    let formRef: HTMLFormElement | null = null;

const SERVICE_ID = import.meta.env.VITE_SERVICE_ID
const TEMPLATE_ID = import.meta.env.VITE_TEMPLATE_ID
const PUBLIC_KEY = import.meta.env.VITE_PUBLIC_KEY

const sendEmail = (e: SubmitEvent) => {
  emailjs
    .sendForm(SERVICE_ID, TEMPLATE_ID, e.target as HTMLFormElement, {
      publicKey: PUBLIC_KEY,
    })
    .then(
      () => {
        console.log('SUCCESS!');
      },
      (error) => {
        console.log('FAILED...', error.text);
      },
    );
};

    onMount(() => {
        const handleClick = (e: Event) => {
            const anchor = e.currentTarget as HTMLAnchorElement;
            e.preventDefault();

            const href = anchor.getAttribute('href');
            if (href) {
                const target = document.querySelector(href);
                if (target) {
                    const offset = 6 * 16; // 4rem in pixels (assuming 1rem = 16px)
                    const targetPosition = target.getBoundingClientRect().top + window.scrollY - offset;

                    window.scrollTo({
                        top: targetPosition,
                        behavior: 'smooth',
                    });
                }
            }
        };

        const anchors = document.querySelectorAll('a[href^="#"]');
        anchors.forEach((anchor) => anchor.addEventListener('click', handleClick));

        return () => {
            anchors.forEach((anchor) => anchor.removeEventListener('click', handleClick));
        };
    });
</script>

<div id="home"></div>
<div class="mx-auto w-[80%] max-w-5xl">
	<div class="mt-8 grid md:h-[80vh] grid-cols-1 md:grid-cols-2 gap-4">
        <div class="flex md:hidden items-center justify-center">
			<img src="/Gabriel.png" alt="Profile" class="m-8 h-auto w-full" />
		</div>
		<div class="flex flex-col items-start justify-center gap-4">
			<p class="text-primary-600 font-medium">Web Developer</p>
			<h1 class="text-4xl font-medium">Hello! My name is Gabriel Schumacher!</h1>
			<p class="text-md">
				I am a recently graduated Web Developer who loves building fast, accessible, and user
				friendly apps! I specialize in frameworks like React, Svelte and Vue. I also have some
				experience working with Wordpress and PHP. I'm currently looking for opportunities to
				contribute to impactful projects and grow as a developer!
			</p>
			<div class="mt-4 flex gap-4">
				<button type="button" class="btn bg-primary-500 w-22 rounded-3xl p-3 shadow-xl"
					><a href="/Gabriel_Resume.pdf" download>Resume</a></button
				>
				<button type="button" class="btn bg-secondary-500 w-22 rounded-3xl p-3 shadow-xl">CV</button
				>
			</div>
		</div>
		<div class="hidden md:flex items-center justify-center">
			<img src="/Gabriel.png" alt="Profile" class="m-8 h-auto w-full p-8" />
		</div>
	</div>
	<h1 id="projects" class="mt-8 text-4xl font-medium text-primary-400-600">Projects</h1>
	<div class="mt-6 grid md:h-[80vh] grid-cols-1 md:grid-cols-2 gap-4">
		<ProjectCard buttonText="React" image="/standin.webp" />
		<ProjectCard buttonText="Svelte" image="/standin.webp" />
		<ProjectCard buttonText="Vue" image="/standin.webp" />
		<ProjectCard buttonText="Wordpress" image="/wordpress.webp" link="http://52.15.162.249/" />
	</div>

	<h1 id="contact" class="mt-6 text-4xl font-medium text-primary-400-600">Contact</h1>  
    <div class="md:h-[80vh]">
        <p>I am very excited about learning new code! I would love the opportunity to learn from your team, and contribute to your projects!</p>

        <form bind:this={formRef} on:submit|preventDefault={sendEmail} class="flex flex-col gap-4 mt-6 card shadow-xl rounded-lg p-4 border-1 border-gray-200">
            <div>
                <label for="user_name">Your Name</label>
                <input name="user_name" class="input" required/>                
            </div>
            <div>
                <label for="user_email">Your Email</label>
                <input name="user_email" class="input"/>                
            </div>
            <div>
                <label for="message">Your Message</label>
                <textarea name="message" class="textarea" value="message" required></textarea>                
            </div>
            <input type="hidden" name="to_email" value="schumachergabe@gmail.com" />
            <input type="submit" value="Send" class="btn bg-primary-500 w-22 rounded-3xl p-3 shadow-xl cursor-pointer" required/>
          </form>     
    </div>

</div>
