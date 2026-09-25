<script lang="ts">
	import BackgroundImage from '$lib/components/BackgroundImage.svelte';
	import { fade } from 'svelte/transition';
	let showCover = $state(true);
	let videoEl1: HTMLVideoElement | undefined = $state();
	let videoEl2: HTMLVideoElement | undefined = $state();
	function handleState1() {
		showCover = false;
		if (videoEl1) {
			videoEl1.muted = false;
			videoEl1.play();
		}
		if (videoEl2) {
			videoEl2.muted = false;
			videoEl2.play();
		}
	}
	$effect(() => {
		document.body.style.overflow = showCover ? 'hidden' : '';
		return () => {
			document.body.style.overflow = '';
		};
	});
</script>

{#if showCover}
	<button
		class="fixed inset-0 z-50 h-dvh w-full text-xs"
		transition:fade={{ duration: 1500 }}
		onclick={handleState1}
	>
		<BackgroundImage src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252474/1.webp" />
		<div class="h-1/2"></div>
		<div class="absolute top-6/10 w-full text-white">
			<div class="mx-auto flex flex-col gap-4 text-center">
				<p>THE WEDDING OF</p>
				<h2 class="text-xl">DWIJAKSARA & WIDYA</h2>
				<p>11 Oktober 2026</p>
			</div>
			<div class="mt-16 flex flex-col gap-2">
				<h2>Kepada Yth.</h2>
				<h2>Recipient</h2>
				<h2>11 Oktober 2026</h2>
			</div>
			<div class="mt-16 text-center">
				<p>Kami memohon maaf</p>
				<p>apabila terjadi kekeliruan</p>
				<p>dalam penulisan nama atau gelar</p>
			</div>
		</div>
	</button>
{/if}

<div class="h-screen snap-y snap-mandatory overflow-y-scroll motion-safe:scroll-smooth">
	<section class="relative h-screen snap-start snap-always bg-black">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252479/countDown.webp"
			class="blur-sm"
		/>
		<div class="absolute top-3/10 left-1/2 z-0 w-9/10 -translate-1/2 overflow-hidden rounded-3xl">
			<video
				class="h-full w-full object-cover object-center"
				src="https://res.cloudinary.com/dzzfgwj4/video/upload/v1789096155/intro.mp4"
				bind:this={videoEl1}
				muted
				loop
				playsinline
			></video>
		</div>
		<div class="absolute top-7/10 left-1/2 z-0 w-9/10 -translate-1/2 overflow-hidden rounded-3xl">
			<video
				class="h-full w-full object-cover object-center"
				src="https://res.cloudinary.com/dzzfgwj4/video/upload/v1789096155/intro.mp4"
				bind:this={videoEl2}
				muted
				loop
				playsinline
			></video>
		</div>
	</section>

	<section class="relative h-screen snap-start snap-always text-sm">
		<div
			class="absolute top-2/9 left-1/2
        z-10 w-6/10 -translate-1/2 px-2
    text-justify text-white"
		>
			<div class="text-justify leading-tight tracking-[3px] [text-align-last:justify]">
				<p>"Ya Tuhan Yang Maha Pen-</p>
				<p>gasih, anugrahkanlah kepa-</p>
				<p>da pasangan ini tanpa ter-</p>
				<p>pisahkan, panjangkan umur,</p>
				<p>semoga pernikahan ini dia-</p>
				<p>anugrahkan putra-putri dan</p>
				<p>cucu yang memberi peng-</p>
				<p>hiburan, tinggal di rumah</p>
				<p>yang penuh kebahagiaan."</p>
			</div>
			<p class="mt-4 text-center font-heading text-lg">Reg Weda X. 85.42</p>
		</div>
		<BackgroundImage src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252474/2.webp" />
	</section>

	<section class="relative h-screen snap-start snap-always">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252474/brideMale.webp"
		/>
		<div class=" absolute top-7/10 z-10 w-full -translate-y-1/2 text-sm text-white">
			<div class="mx-auto flex flex-col text-center">
				<h2 class="mb-8 font-heading text-xl">The Groom</h2>
				<p class="-mb-1 text-lg">I Gusti Bagus Agung</p>
				<p class="mb-4 text-lg">Dwijaksara, S.Arsl., IALI</p>
				<h2 class="mb-8 font-heading text-xl">The Second Son of</h2>
				<p>I Gusti Ngurah Kanca</p>
				<p>& Ida Ayu Puja Widiastuti</p>
			</div>
		</div>
	</section>

	<section class="relative h-screen snap-start snap-always">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252474/brideFemale.webp"
		/>
		<div class=" absolute top-7/10 z-10 w-full -translate-y-1/2 text-sm text-white">
			<div class="mx-auto flex flex-col text-center">
				<h2 class="mb-8 font-heading text-xl">The Groom</h2>
				<p class="-mb-1 text-lg">Ni Putu Widya Maheswari</p>
				<p class="mb-4 text-lg">Karantika Putri, S.Par., M.Tr.Par</p>
				<h2 class="mb-8 font-heading text-xl">The Second Son of</h2>
				<p class="tracking-wide">Dr. I ketut Kanten, A.Par., S.E., M.M., CHA., CHE.,</p>
				<p>Ni Komang Kartika, S.E</p>
			</div>
		</div>
	</section>

	<section class="relative h-screen snap-start snap-always">
		<BackgroundImage src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252474/6.webp" />
		<div
			class="absolute top-4/20 left-1/2
        z-10 w-4/5 -translate-1/2 px-2
    text-justify text-white"
		>
			<div
				class="  text-justify text-sm
      leading-normal tracking-widest"
			>
				<p>Like roots beneath the earth, love</p>
				<p>grows quietly, deeply, and with purpose.</p>
				<p>Through every season, we choose to grow</p>
				<p>together, to nurture one another, and to</p>
				<p>build a life grounded in love, trust, and</p>
				<p>togetherness.</p>
				<p class="mt-4">With joyful hearts, we invite you to wit-</p>
				<p>ness and celebrate the beginning of our</p>
				<p>next chapter together.</p>
			</div>
		</div>
		<div
			class="absolute top-7/11 left-1/2
        z-10 w-4/5 -translate-1/2 px-2
    text-center text-white"
		>
			<h2 class="font-heading text-3xl">Dwijaksara & Widya</h2>
		</div>
	</section>

	<section class="relative h-screen snap-start snap-always">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252474/locationDate.webp"
		/>
		<div
			class="absolute top-7/12 left-1/2
        z-10 w-4/5 -translate-x-1/2 px-2
    text-center text-white"
		>
			<h2 class="mb-14 font-heading text-3xl">Save Our Date</h2>
			<p class="mb-2 text-lg">11 Oktober 2026</p>
			<div class="mb-6">
				<p class="-mb-1 text-sm">Wedding Reception</p>
				<p class="text-lg">Sunday, 17.00 - End</p>
			</div>
			<p class="mb-2 text-lg">Bali Wangi Venue</p>
			<div class="mb-2 text-sm">
				<p class="-mb-1">Kesiman, Kec Denpasar Timur,</p>
				<p>Kota Denpasar, Bali</p>
			</div>
			<h2 class="font-heading text-xl">Google Maps</h2>
		</div>
	</section>

	<section class="relative h-screen snap-start snap-always font-heading text-2xl">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252474/countDown.webp"
		/>
		<div
			class="absolute top-7/12 left-1/2
        z-10 w-4/5 -translate-1/2 px-2
    text-justify text-white"
		>
			<div class="mb-20 text-center">
				<h3 class="mb-2">Time For Our</h3>
				<h2 class="text-3xl">Celebration</h2>
			</div>
			<div class="relative mb-8 flex items-center justify-between">
				<div class="flex flex-col gap-8 text-center">
					<h3>1</h3>
					<h2>Days</h2>
				</div>
				<div class="flex flex-col gap-8 text-center">
					<h3>1</h3>
					<h2>Minutes</h2>
				</div>
				<div
					class=" absolute top-1/2 left-1/2 flex
      -translate-1/2 flex-col gap-8 text-center"
				>
					<h3>1</h3>
					<h2>Hours</h2>
				</div>
			</div>
			<div class="flex flex-col gap-8 text-center">
				<h3>1</h3>
				<h2>Seconds</h2>
			</div>
		</div>
	</section>

	<section class="relative h-screen snap-start snap-always">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252475/attendanceConfirmation.webp"
		/>
		<div class=""></div>
	</section>

	<section class="relative h-screen snap-start snap-always">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252472/wishes.webp"
		/>
	</section>

	<section class="relative h-screen snap-start snap-always text-sm text-white">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252474/hadiahDigital.webp"
		/>
		<div
			class="absolute top-1/6 flex
    w-full flex-col"
		>
			<h2 class="mb-12 text-center font-heading text-xl">Hadiah Digital</h2>
			<div class="mx-auto text-justify leading-tight tracking-widest [text-align-last:justify]">
				<p>Kehadiran dan doa restu Anda merupakan</p>
				<p>hadiah terindah bagi kami. Namun, bagi</p>
				<p>yang berkenan memberikan tanda kasih, dapat</p>
				<p>disampaikan melalui rekening berikut</p>
			</div>
		</div>
	</section>

	<section class="relative h-dvh w-full snap-start snap-always text-sm text-white">
		<BackgroundImage src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252474/last.webp" />
		<div class="h-1/2"></div>
		<div class="relative z-10 h-1/2 text-white">
			<div class="mx-auto flex flex-col text-center">
				<p>THE WEDDING OF</p>
				<h2 class="mb-8 font-heading text-3xl">Dwijaksara & Widya</h2>
				<p>11 Oktober 2026</p>
			</div>
		</div>
		<div class="absolute top-17/20 w-full translate-y-1/2 text-center text-base">
			<h1>THANK YOU</h1>
			<h1>FOR YOUR ATTENDANCE</h1>
		</div>
	</section>
</div>
