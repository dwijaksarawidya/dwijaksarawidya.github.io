<script lang="ts">
	import BackgroundImage from '$lib/components/BackgroundImage.svelte';
	import { fade, scale } from 'svelte/transition';
	import initialWishes from '$lib/data/wishes.json';
	import ChevronRight from '@lucide/svelte/icons/chevron-right';
	import ChevronLeft from '@lucide/svelte/icons/chevron-left';

	//WISH
	const API_URL =
		'https://script.google.com/macros/s/AKfycbx8SHAWZtHAUFVQ_OPhs7YForFev_hbpAsIqqz2ITU1AIsuKzkc_qZ6mw6VBEyMFlue/exec';

	let wishes = $state(initialWishes);
	let wishLoading = $state(false);
	let name = $state('');
	let wish = $state('');
	let guestNum = $state(0);
	let attend = $state('');

	let sending = $state(false);
	let sendingError = $state('');

	async function loadWishes() {
		wishLoading = true;
		try {
			const res = await fetch(API_URL);
			const json = await res.json();
			wishes = json.data.map((r) => ({ name: r.name, wish: r.wish }));
		} catch (e) {
			console.error(e);
		} finally {
			wishLoading = false;
		}
	}

	async function addWish(name: string, wish: string, guestNum: number, attend: string) {
		wishes.push({ name, wish });
		fetch(API_URL, {
			method: 'POST',
			headers: { 'Content-Type': 'text/plain;charset=utf8' },
			body: JSON.stringify({ sheet: 'Sheet1', data: { name, wish, guestNum, attend } })
		});
	}
	async function handleWishSubmit(e: SubmitEvent) {
		e.preventDefault();
		if (!name.trim() || !wish.trim()) return;
		const item = {
			name: name.trim(),
			wish: wish.trim(),
			guestNum: guestNum,
			attend: attend.trim()
		};
		sending = true;
		sendingError = '';
		const added = wishes[wishes.length - 1];
		try {
			await addWish(item.name, item.wish, item.guestNum, item.attend);
			name = '';
			wish = '';
		} catch (err) {
			wishes = wishes.filter((w) => w !== added);
			sendingError = 'Could not send your wish. Please try again';
		} finally {
			sending = false;
		}
	}

	$effect(() => {
		loadWishes();
	});
	//WISH

	// rekening
	let rekening = $state(false);
	function handleRekening() {
		rekening = rekening ? false : true;
	}
	let noRekening = '1777720230';
	let copiedRekening = $state(false);
	async function copyRekening() {
		try {
			await navigator.clipboard.writeText(noRekening);
			copiedRekening = true;
			setTimeout(() => (copiedRekening = false), 2000);
		} catch (err) {
			console.error('failed to copy:', err);
		}
	}
	// rekening

	// Cover dan Video
	let showCover = $state(true);
	let videoEl1: HTMLVideoElement | undefined = $state();
	function handleState1() {
		showCover = false;
		if (videoEl1) {
			videoEl1.muted = false;
			videoEl1.play();
		}
	}
	$effect(() => {
		document.body.style.overflow = showCover ? 'hidden' : '';
		return () => {
			document.body.style.overflow = '';
		};
	});
	// Cover dan Video

	// countDown
	let days = $state(0);
	let hours = $state(0);
	let minutes = $state(0);
	let seconds = $state(0);

	function getTimeRemaining() {
		const target = new Date('2026-10-09T09:00:00').getTime();
		const now = Date.now();
		const diff = Math.max(target - now, 0);

		return {
			days: Math.floor(diff / (1000 * 60 * 60 * 24)),
			hours: Math.floor((diff / (1000 * 60 * 60)) % 24),
			minutes: Math.floor((diff / (1000 * 60)) % 60),
			seconds: Math.floor((diff / 1000) % 60)
		};
	}
	$effect(() => {
		function update() {
			const t = getTimeRemaining();
			days = t.days;
			hours = t.hours;
			minutes = t.minutes;
			seconds = t.seconds;
		}

		update();
		const interval = setInterval(update, 1000);
		return () => clearInterval(interval);
	});
	// countDown

	//recipient
	let recipient1 = $state('Bapak/Ibu/Saudara/i');
	let recipient2 = $state<string | null>(null);
	$effect(() => {
		const params = new URLSearchParams(window.location.search);
		recipient1 = params.get('to1') ?? params.get('to') ?? 'Bapak/Ibu/Saudara/i';
		recipient2 = params.get('to2');
	});
	//recipient

	//galeri

	let scrollerGalery = $state<HTMLDivElement>();
	let scrollIndex = $state(0);

	let scroller = $state<HTMLDivElement>();
	let index = $state(0);

	function goTo(i: number) {
		if (!scroller) return;
		const next = (i + photos.length) % photos.length;
		scroller.scrollTo({ left: next * scroller.clientWidth, behavior: 'smooth' });
	}

	function handleScroll(e: Event) {
		const el = e.currentTarget as HTMLDivElement;
		index = Math.round(el.scrollLeft / el.clientWidth);
	}
	const photos = [
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/1.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/2.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/3.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/4.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/5.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/6.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/7.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/8.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/9.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/10.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/11.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/12.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/13.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/14.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/15.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/16.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/17.webp',
		'https://res.cloudinary.com/dzzfgwj4/image/upload/v1790671745/18.webp'
	];
	//galeri
</script>

{#if showCover}
	<div
		class="fixed top-0 z-50 h-dvh w-full max-w-[430px] overflow-hidden text-xs"
		transition:fade={{ duration: 1500 }}
	>
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790674190/neoCover.webp"
		/>
		<div class="h-1/2"></div>
		<div class="absolute top-5/10 w-full text-white">
			<div class="mx-auto flex flex-col gap-1 text-center">
				<p>THE WEDDING OF</p>
				<h2 class="mb-10 font-heading text-3xl">Dwijaksara & Widya</h2>
				<p>09 Oktober 2026</p>
			</div>
			<div class="mt-16 flex flex-col gap-2 text-center">
				<h2 class=" text-[9px] font-extralight">Kepada Yth.</h2>
				<div class="text-center font-black">
					<h2>{recipient1}</h2>
					{#if recipient2}
						<h2>{recipient2}</h2>
					{/if}
				</div>
			</div>
		</div>
		<button
			onclick={handleState1}
			class=" absolute bottom-20 left-1/2 -translate-x-1/2 font-black
      text-white underline shadow-2xl transition-all duration-100 ease-out active:scale-98"
		>
			BUKA UNDANGAN
		</button>
		<div class="absolute bottom-6 w-full text-center text-[9px] text-white">
			<p>Kami memohon maaf apabila terjadi</p>
			<p>kekeliruan dalam penulisan nama atau gelar</p>
		</div>
	</div>
{/if}

<div
	class="fixed h-dvh max-w-[430px] snap-y snap-mandatory overflow-x-hidden overflow-y-scroll motion-safe:scroll-smooth
  "
>
	<section
		class="relative flex h-full snap-start snap-always flex-col
    items-center overflow-hidden bg-black"
	>
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790674190/neoCover.webp"
			class="scale-150 blur-xs"
		/>
		<div class="flex-1"></div>

		<div
			class=" relative z-0 w-9/10 overflow-hidden
		    shadow-2xl shadow-black/90"
		>
			<video
				class="h-full w-full object-cover object-center"
				src="https://res.cloudinary.com/dzzfgwj4/video/upload/v1789096155/intro.mp4"
				bind:this={videoEl1}
				muted
				loop
				playsinline
				disablepictureinpicture
			></video>
		</div>
		<div class="relative w-full flex-1 text-center text-sm text-white">
			<p class="mt-4">I'll take you home,</p>
			<p>and spend a lifetime making it ours.</p>
		</div>
	</section>

	<section class="relative h-full snap-start snap-always text-sm">
		<div
			class="absolute top-2/9 left-1/2
        z-10 w-[258px] -translate-1/2 px-2
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
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790674191/neoPuisi.webp"
		/>
	</section>

	<section class="relative h-full snap-start snap-always">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790528504/brideMale.webp"
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

	<section class="relative h-full snap-start snap-always">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790528513/brideFemale.webp"
		/>
		<div class=" absolute top-7/10 z-10 w-full -translate-y-1/2 text-sm text-white">
			<div class="mx-auto flex flex-col text-center">
				<h2 class="mb-8 font-heading text-xl">The Bride</h2>
				<p class="-mb-1 text-lg">Ni Putu Widya Maheswari</p>
				<p class="mb-4 text-lg">Karantika Putri, S.Par., M.Tr.Par</p>
				<h2 class="mb-8 font-heading text-xl">The First Daughter of</h2>
				<p class="tracking-wide">Dr. I Ketut Kanten, A.Par., S.E., M.M</p>
				<p>Ni Komang Kartika, S.E</p>
			</div>
		</div>
	</section>

	<section class="relative h-full snap-start snap-always">
		<BackgroundImage src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317218/3-3.jpg" />
		<div
			class="absolute top-4/20 left-1/2
        z-10 w-[344px] -translate-1/2
      px-2 text-white"
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

	<section class="relative h-full snap-start snap-always">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252474/galeri2.webp"
		/>
		<div
			class="absolute top-1/7 left-1/2
        z-20 w-4/5 -translate-x-1/2 -translate-y-10 px-2
    text-center text-white"
		>
			<h2 class="mb-14 font-heading text-3xl">Save Our Date</h2>
			<p class="mb-2 text-lg">09 Oktober 2026</p>
			<div class="mb-6">
				<p class="-mb-1 text-sm">Pawiwahan</p>
				<p class="text-lg">Friday, 09.00 - 20.00</p>
			</div>
			<p class="mb-2 text-lg">Jero Tegal Sanur</p>
			<div class="mb-2 text-sm">
				<p class="-mb-1">Jl. Sekuta Gg. VII No. 9B Sanur, Denpasar, Bali</p>
				<p>Kota Denpasar, Bali</p>
			</div>
			<a
				href="https://maps.app.goo.gl/voomPTjma4eGBA4E9?g_st=iw"
				class="inline-flex shadow-2xl transition-all duration-100 ease-out active:scale-98"
				target="_blank"
				rel="noopener noreferrer"
			>
				<h2 class="font-heading text-lg underline">Google Maps</h2>
			</a>
		</div>
	</section>

	<section class="relative h-full snap-start snap-always font-heading text-2xl">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790528503/countDown.webp"
		/>
		<div
			class="absolute top-1/7 left-1/2
        z-10 w-4/5 -translate-x-1/2 -translate-y-15 px-2
    text-justify text-white"
		>
			<div class="mb-20 text-center">
				<h3 class="mb-2">Time For Our</h3>
				<h2 class="text-3xl">Celebration</h2>
			</div>
			<div class="relative mb-8 flex items-center justify-between">
				<div class="flex flex-col gap-8 text-center">
					<h3>{days}</h3>
					<h2>Days</h2>
				</div>
				<div class="flex flex-col gap-8 text-center">
					<h3>{minutes}</h3>
					<h2>Minutes</h2>
				</div>
				<div
					class=" absolute top-1/2 left-1/2 flex
      -translate-1/2 flex-col gap-8 text-center"
				>
					<h3>{hours}</h3>
					<h2>Hours</h2>
				</div>
			</div>
			<div class="flex flex-col gap-8 text-center">
				<h3>{seconds}</h3>
				<h2>Seconds</h2>
			</div>
		</div>
	</section>

	<section class="relative flex h-full snap-start snap-always flex-col justify-center">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790252472/wishes.webp"
		/>
		<div class="absolute inset-0 bg-black/50"></div>
		<div class="relative z-10 flex h-8/10 flex-col gap-15 text-center text-sm text-white">
			<h2 class="font-heading text-xl">Wishes</h2>
			<div class="mx-4 flex-1 overflow-y-auto">
				{#each wishes as wish}
					<div class="mb-8">
						<p class="mb-2 font-black">{wish.name}</p>
						<p class="text-xs">{wish.wish}</p>
					</div>
				{/each}
			</div>
		</div>
	</section>

	<section class="relative flex h-full snap-start snap-always flex-col justify-center">
		<BackgroundImage
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790625545/formWishes.webp"
		/>
		<div class="absolute inset-0 bg-black/70"></div>
		<div class="relative z-10 flex h-9/10 flex-col gap-10 text-center text-sm text-white">
			<div>
				<h1>Kindly Confirm Your</h1>
				<h1>Presence And Share Your Blessings</h1>
			</div>
			<form class="flex flex-1 flex-col gap-6 px-8 text-center" onsubmit={handleWishSubmit}>
				<fieldset class="">
					<h3 class="mb-6 font-heading text-xl">Name</h3>
					<input
						type="text"
						placeholder="Your name"
						bind:value={name}
						required
						class="w-full bg-black/80 py-2 text-center"
					/>
				</fieldset>
				<fieldset>
					<h3 class="mb-6 font-heading text-xl">Attendance Confirmation</h3>
					<div class="flex justify-center gap-4">
						<label>
							<input type="radio" value="Yes" bind:group={attend} required />
							Yes
						</label>
						<label>
							<input type="radio" value="No" bind:group={attend} required />
							No
						</label>
					</div>
				</fieldset>
				<fieldset>
					<h3 class="mx-4 mb-6 font-heading text-xl">Guest Attendance</h3>
					<input
						type="text"
						inputmode="numeric"
						class="w-20 bg-black/80 py-2 text-center"
						bind:value={guestNum}
						oninput={(e) => {
							const digits = e.currentTarget.value.replace(/\D/g, '');
							guestNum = digits === '' ? '' : Math.min(20, Math.max(1, Number(digits)));
							e.currentTarget.value = String(guestNum);
						}}
					/>
				</fieldset>
				<fieldset>
					<h3 class="mb-6 font-heading text-xl">Wishes</h3>
					<textarea
						placeholder="Your wish for bride and groom"
						bind:value={wish}
						rows="4"
						class="w-full resize-none bg-black/80 py-2 text-center"></textarea>
				</fieldset>
				<button
					class="mx-auto mt-8 w-1/3 font-heading text-2xl
        underline shadow-2xl transition-all duration-100 ease-out active:scale-98"
					type="submit"
					disabled={sending}>{sending ? 'Sending...' : 'Submit'}</button
				>
			</form>
		</div>
	</section>

	<section class="relative h-full snap-start snap-always text-sm text-white">
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
				<p>disampaikan melalui rekening berikut:</p>
			</div>
			<button
				onclick={handleRekening}
				class="shadow-2xl transition-all duration-100 ease-out active:scale-98"
			>
				<h2 class="mt-8 text-center font-heading text-2xl underline">Click Here</h2>
			</button>
			{#if rekening}
				<div
					class="mx-auto mt-8 bg-black/80 px-10 py-6 text-center"
					transition:scale={{ duration: 250, start: 0.9 }}
				>
					<p>Ni Putu Widya Maheswari</p>
					<span>MAYBANK</span>
					<span>{noRekening}</span>
				</div>
			{/if}
		</div>
	</section>

	<section class="relative h-full snap-start snap-always text-sm text-white">
		<div
			bind:this={scroller}
			onscroll={handleScroll}
			class="flex h-full w-full snap-x snap-mandatory [scrollbar-width:none] overflow-x-auto [&::-webkit-scrollbar]:hidden"
		>
			{#each photos as src, i}
				<img
					{src}
					alt="Photo {i + 1}"
					loading={i === 0 ? 'eager' : 'lazy'}
					class="h-full w-full shrink-0 snap-center object-cover"
				/>
			{/each}
		</div>

		<!-- <div class="flex"></div> -->
		<!-- <BackgroundImage -->

		<button
			type="button"
			onclick={() => goTo(index - 1)}
			aria-label="Previous photo"
			class="absolute top-1/2 left-3 flex h-6 w-6 -translate-y-1/2 items-center justify-center rounded-full bg-white/30 text-2xl text-white
    transition-all duration-700 ease-out active:scale-110"
		>
			<ChevronLeft size={12} />
		</button>

		<!-- next -->

		<button
			type="button"
			onclick={() => goTo(index + 1)}
			aria-label="Next photo"
			class="absolute top-1/2 right-3 flex h-6 w-6 -translate-y-1/2 items-center justify-center rounded-full bg-white/30 text-2xl text-white
    transition-all duration-100 ease-out active:scale-110"
		>
			<ChevronRight size={12} />
		</button>

		<!-- counter -->
		<p
			class="absolute bottom-4 left-1/2 -translate-x-1/2 rounded-full bg-white/20 px-3 py-1 text-xs text-white"
		>
			{index + 1} / {photos.length}
		</p>
		<div class="absolute top-1/7 w-full -translate-y-1/2">
			<h3 class="z-20 text-center font-heading text-2xl font-black">A Gallery of Us</h3>
		</div>
	</section>

	<section class="relative h-full w-full snap-start snap-always text-sm text-white">
		<BackgroundImage src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1790529008/last.webp" />
		<div class="absolute inset-0 bg-black/50"></div>
		<div class="h-1/2"></div>
		<div class="relative z-10 h-1/2 text-white">
			<div class="mx-auto flex flex-col gap-6 text-center text-xs">
				<p>THE WEDDING OF</p>
				<h2 class="mb-8 font-heading text-3xl">Dwijaksara & Widya</h2>
				<p>09 Oktober 2026</p>
			</div>
		</div>
		<div class="absolute top-16/20 w-full translate-y-1/2 text-center text-lg">
			<h1>THANK YOU</h1>
			<h1 class="-mt-1.5">FOR YOUR ATTENDANCE</h1>
		</div>

		<div class="absolute bottom-4 z-20 mx-auto w-full text-center text-[8px]">
			<p>CREATED BY MANUSKRIP</p>
			<a
				class="inline-flex shadow-2xl transition-all duration-100 ease-out active:scale-98"
				href="https://wa.me/6289678495387"
				target="_blank"
				rel="noopener noreferrer"
			>
				<p>+6289678495387</p>
			</a>
		</div>
	</section>
</div>
