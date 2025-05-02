<script lang="ts">
	import ProjectCard from '$lib/components/ProjectCard.svelte';
	import { onMount } from 'svelte';
	import emailjs from '@emailjs/browser';

	import { Pagination } from '@skeletonlabs/skeleton-svelte';

	interface CardData {
		buttonText: string;
		image: string;
		link: string;
	}

	let cardData: CardData[] = [
		{ 
            buttonText: 'React', 
            image: '/standin.webp', 
            link: '' },
		{ 
            buttonText: 'Svelte', 
            image: '/standin.webp', 
            link: '' },
		{ 
            buttonText: 'Vue', 
            image: '/standin.webp', 
            link: '' },
		{
			buttonText: 'I created a custom Wordpress theme for a school project. I used PHP and SCSS to create a mobile friendly design. I can also manage ecommerce in Wordpress sites using WooCommerce.',
			image: '/wordpress.webp',
			link: 'http://52.15.162.249/'
		},
		{
			buttonText: 'I coded based on what I was given by the design team. We were tasked to create desktop landing page for the Candy Bombers website. I used vanilla JS, HTML and CSS.',
			image: '/candy.webp',
			link: 'https://prototype-candy-bomber.netlify.app/home'
		},
        {
			buttonText: 'As a team we were tasked to create a redesign prototype for the SLSS department at UVU. I took it a step further and created a fully functional website.',
			image: '/slss.webp',
			link: 'https://tangerine-duckanoo-61cabe.netlify.app/'
		},
        {
			buttonText: 'As a hobby I like to create mods! I created one for the 2003 game Command and Conquer Generals. It was what initially got me into coding and helped me pick a career!',
			image: '/mod.webp',
			link: 'https://www.moddb.com/mods/zero-hour-escalated'
		}
	];

	let page = 1;
	let size = 4;

	$: slicedSource = cardData.slice((page - 1) * size, page * size);

	let formRef: HTMLFormElement | null = null;

	const SERVICE_ID = import.meta.env.VITE_SERVICE_ID;
	const TEMPLATE_ID = import.meta.env.VITE_TEMPLATE_ID;
	const PUBLIC_KEY = import.meta.env.VITE_PUBLIC_KEY;

	const sendEmail = (e: SubmitEvent) => {
		emailjs.sendForm(SERVICE_ID, TEMPLATE_ID, e.target as HTMLFormElement, PUBLIC_KEY).then(
			() => {
				console.log('SUCCESS!');
			},
			(error) => {
				console.log('FAILED...', error.text);
			}
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
						behavior: 'smooth'
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
	<div class="mt-8 grid grid-cols-1 gap-4 md:h-[80vh] md:grid-cols-2">
		<div class="flex items-center justify-center md:hidden">
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
		<div class="hidden items-center justify-center md:flex">
			<img src="/Gabriel.png" alt="Profile" class="m-8 h-auto w-full p-8" />
		</div>
	</div>
	<h1 id="projects" class="text-primary-400-600 mt-8 mb-2 text-4xl font-medium">Projects</h1>
    <div class="grid gap-4 grid-cols-1 md:grid-cols-2 md:min-h-[80vh]">

            {#each slicedSource as card}
                <ProjectCard {...card} />
            {/each}           

        <div class="w-full flex justify-center md:col-span-2">
            <Pagination
                data={cardData}
                {page}
                onPageChange={(e) => (page = e.page)}
                pageSize={size}
                onPageSizeChange={(e) => (size = e.pageSize)}
                siblingCount={4}
                classes="flex justify-center gap-4 align-top h-14"
                buttonClasses="btn bg-primary-500 w-22 rounded p-3 shadow-xl"
                buttonActive="bg-secondary-500 text-white"
            />            
        </div>

	</div>


	<h1 id="contact" class="text-primary-400-600 mt-6 text-4xl font-medium">Contact</h1>
	<div class="md:min-h-[80vh]">
		<p>
			I am very excited about learning new code! I would love the opportunity to learn from your
			team, and contribute to your projects! I am still new to development, but with me you will get a motivated learner who will work hard so you can spend more time doing what you need to do!
		</p>

		<form
			bind:this={formRef}
			on:submit|preventDefault={sendEmail}
			class="card mt-6 flex flex-col gap-4 rounded-lg border-1 border-gray-200 p-4 shadow-xl"
		>
			<div>
				<label for="user_name">Your Name</label>
				<input name="user_name" class="input" required />
			</div>
			<div>
				<label for="user_email">Your Email</label>
				<input name="user_email" class="input" />
			</div>
			<div>
				<label for="message">Your Message</label>
				<textarea name="message" class="textarea" placeholder="Your message here..." required
				></textarea>
			</div>
			<input type="hidden" name="to_email" value="schumachergabe@gmail.com" />
			<input
				type="submit"
				value="Send"
				class="btn bg-primary-500 w-22 cursor-pointer rounded-3xl p-3 shadow-xl"
				required
			/>
		</form>
	</div>
</div>
