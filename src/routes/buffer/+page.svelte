<script lang="ts">
	import HeaderFooter from '$lib/components/headerFooter.svelte';
	import InfoPengantin from '$lib/components/InfoPengantin.svelte';
	import Cover from '$lib/components/Cover.svelte';
	import { blur, fade, fly } from 'svelte/transition';
	import { page } from '$app/state';
	let showCover = $state(true);
	let showEnvelope = $state(true);
	let videoEl: HTMLVideoElement | undefined = $state();

	function handleState1() {
		showCover = false;
		if (videoEl) {
			videoEl.muted = false;
			videoEl.play();
		}
	}

	$effect(() => {
		document.body.style.overflow = showCover || showEnvelope ? 'hidden' : '';
		return () => {
			document.body.style.overflow = '';
		};
	});
	let recipient1 = $state('Bapak/Ibu/Saudara/i');
	let recipient2 = $state<string | null>(null);

	$effect(() => {
		const params = new URLSearchParams(window.location.search);
		recipient1 = params.get('to1') ?? params.get('to') ?? 'Bapak/Ibu/Saudara/i';
		recipient2 = params.get('to2');
	});
</script>

<!-- state1  -->
{#if showCover}:console.warn();

	<button
		class="fixed top-0 left-1/2 z-10 mx-auto h-dvh w-full max-w-[430px] -translate-x-1/2"
		transition:fade={{ duration: 1500 }}
		onclick={handleState1}
	>
		<div transition:blur={{ duration: 500, opacity: 200 }}>
			<Cover></Cover>
		</div>
	</button>
{/if}
<!-- state2 -->
{#if showEnvelope}
	<button
		class="fixed inset-0 z-9 mx-auto h-dvh w-full max-w-[430px] overflow-hidden"
		transition:fade={{ duration: 1000 }}
		onclick={() => (showEnvelope = false)}
	>
		<div
			class="absolute top-1/2 left-1/2 aspect-34/9 w-5/6 -translate-x-1/2 -translate-y-1/2 font-body text-[10px] text-white shadow-[0_0_0_9999px_var(--color-primary)]"
		>
			<div
				class="bottom-full-full absolute left-0 -mt-7 flex w-full items-center
      justify-center text-center"
			>
				<h1 class="" out:fly={{ y: -200, duration: 2000 }}>BUKA UNDANGAN</h1>
			</div>
			<div
				class="absolute top-full left-0 mt-5 flex w-full flex-col items-center
      justify-center gap-0 text-center"
				out:fly={{ y: 200, duration: 2000 }}
			>
				<h1 class="">{recipient1}</h1>
				{#if recipient2}
					<h1 class="">{recipient2}</h1>
				{/if}
			</div>
		</div>
		<div
			class="absolute top-1/4 left-0 flex w-full -translate-y-1/2 items-center
      justify-center text-center font-heading text-base text-white"
		>
			<h1 class="" out:fly={{ y: -200, duration: 1200 }}>Dwijaksara & Widya</h1>
		</div>
		<HeaderFooter variant="2" position="bottom"></HeaderFooter>
	</button>
{/if}
<!-- state3 -->
<div
	class="fixed inset-0 z-20 hidden
  items-center justify-center bg-black p-8 text-center font-body
  text-white [@media(max-height:500px)]:flex"
>
	<p class="text-xl">Please rotate your device to portrait mode to view this invitation...</p>
</div>
<div class="  [@media(max-height:500)]:hidden">
	<!--invitation page1 -->
	<div class="relative h-dvh snap-always">
		<div class="relative h-1/2 bg-primary">
			<HeaderFooter variant="1"></HeaderFooter>
			<div
				class="absolute top-4/9
        left-1/2 w-[162px] -translate-1/2 px-2 text-justify text-white"
			>
				<h1 class="mb-6 text-center font-heading text-lg">Om Swastyastu</h1>
				<div class="text-justify leading-2.5 tracking-[1.75px] [text-align-last:justify]">
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
				<p class="mt-2 text-center">(Reg Weda X. 85.42)</p>
			</div>
		</div>
		<div class="h-1/2 bg-white"></div>
		<div class="absolute top-1/2 left-1/2 aspect-video w-5/6 -translate-x-1/2 -translate-y-1/2">
			<div class="aspect-video w-full overflow-hidden">
				<video
					class="h-full w-full object-cover object-center"
					src="https://res.cloudinary.com/dzzfgwj4/video/upload/v1789096155/intro.mp4"
					bind:this={videoEl}
					muted
					loop
					playsinline
				></video>
			</div>
			<div class="absolute top-full left-0 mt-3 w-full">
				<div class=" w-[198px] text-justify leading-3 tracking-[1.3px] text-primary">
					Atas Asung Kertha Wara Nugraha Ida Sang Hyang Widhi Wasa/Tuhan Yang Maha Esa, Kami
					Bermaksud Mengundang Bapak/Ibu/Saudara/i, pada Acara Manusa Yadnya - Pawiwahan anak kami.
				</div>
			</div>
		</div>
	</div>

	<!-- invitation page2 -->
	<div class="h-dvh snap-always">
		<InfoPengantin
			name="I Gusti Bagus Agung Dwijaksara, S.Arsl., IALI"
			orderText="putra ke dua dari pasangan"
			fatherName="I Gusti Ngurah Kanca"
			motherName="Ida Ayu Puja Widiastuti"
			photoSrc="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789493820/pengantinPria.jpg"
			bgSrc="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317205/belum.jpg"
		/>
		<InfoPengantin
			name="Ni Putu Widya Maheswari Karantika Putri, S.Par., M.Tr.Par"
			orderText="putri pertama dari pasangan"
			fatherName="Dr. I ketut Kanten, A.Par., S.E., M.M., CHA., CHE.,"
			motherName="Ni Komang Kartika, S.E"
			photoSrc="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317205/pengantin2.jpg"
			bgSrc="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317205/pengantin2bg.jpg"
		/>
	</div>

	<!-- invitation page3 -->
	<div class="mt-10 h-dvh">
		<div class="mx-6 h-1/2">
			<div class="flex w-full gap-5">
				<!-- Left column: photo 1 + caption -->
				<div class="flex w-1/3 flex-col">
					<div class="aspect-square overflow-hidden">
						<img
							src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317217/3-1.jpg"
							class="h-full w-full object-cover"
							alt="pengantin"
						/>
					</div>
					<div class="w-[115px]">
						<p class="mt-3 -mr-2 text-left leading-2.5 tracking-[1.2px]">
							Apabila Bapak/Ibu/Saudara/i berkenan hadir dan memberikan doa restu kepada kami
						</p>
					</div>
				</div>

				<!-- Right column: intro text, then photo row -->
				<div class="flex w-2/3 flex-col text-left leading-2.5 tracking-[1.2px]">
					<p>
						Merupakan suatu kehormatan dan kebahagiaan bagi kami sekeluarga pada acara sederhana ini
					</p>

					<div class="mt-4 flex gap-5">
						<div class="my-auto">
							<div class="aspect-[7/8] flex-1 overflow-hidden">
								<img
									src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317217/3-2.jpg"
									class="h-full w-full object-cover object-center"
									alt="pengantin"
								/>
							</div>
						</div>
						<div>
							<div class="aspect-[3/4] flex-1 overflow-hidden">
								<img
									src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317217/3-3.jpg"
									class="h-full w-full object-cover object-center"
									alt="pengantin"
								/>
							</div>
						</div>
					</div>
				</div>
			</div>

			<div class="mt-3 flex w-full gap-5">
				<div class="flex w-1/3 flex-col">
					<div class="aspect-square overflow-hidden">
						<img
							src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317217/3-4.jpg"
							class="h-full w-full object-cover"
							alt="pengantin"
						/>
					</div>
				</div>

				<div class="flex w-2/3 flex-col text-left leading-2.5 tracking-[1.2px]">
					<div class="my-auto flex gap-5">
						<div class="">
							<div class="aspect-square overflow-hidden">
								<img
									src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317217/3-5.jpg"
									class="h-full w-full object-cover object-center"
									alt="pengantin"
								/>
							</div>
						</div>
						<div>
							<div class="aspect-square overflow-hidden">
								<img
									src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317217/3-6.jpg"
									class="h-full w-full object-cover object-center"
									alt="pengantin"
								/>
							</div>
						</div>
					</div>
				</div>
			</div>

			<!-- <div class="mt-10 flex gap-5"> -->
			<!-- 	<div class="flex w-1/3"> -->
			<!-- 		<div class="h-full w-full"> -->
			<!-- 			<div class="aspect-square overflow-hidden"> -->
			<!-- 				<img -->
			<!-- 					src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317217/3-4.jpg" -->
			<!-- 					class="h-full w-full object-cover" -->
			<!-- 					alt="pengantin" -->
			<!-- 				/> -->
			<!-- 			</div> -->
			<!-- 		</div> -->
			<!-- 	</div> -->
			<!---->
			<!-- 	<div class="flex w-2/3 flex-col"> -->
			<!-- 		<div class="flex h-full gap-5"> -->
			<!-- 			<div class="flex-1"> -->
			<!-- 				<div class="aspect-square h-full w-full overflow-hidden"> -->
			<!-- 					<img -->
			<!-- 						src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317217/3-5.jpg" -->
			<!-- 						class="h-full w-full object-cover object-center" -->
			<!-- 						alt="pengantin" -->
			<!-- 					/> -->
			<!-- 				</div> -->
			<!-- 			</div> -->
			<!-- 			<div class="flex-1"> -->
			<!-- 				<div class="aspect-square h-full w-full overflow-hidden"> -->
			<!-- 					<img -->
			<!-- 						src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317217/3-6.jpg" -->
			<!-- 						class="h-full w-full object-cover object-center" -->
			<!-- 						alt="pengantin" -->
			<!-- 					/> -->
			<!-- 				</div> -->
			<!-- 			</div> -->
			<!-- 		</div> -->
			<!-- 	</div> -->
			<!-- </div> -->
		</div>
		<div class="relative h-1/2 bg-primary">
			<div class="absolute top-1/5 left-0 w-full text-center text-white">
				<div class="mb-12 flex flex-col gap-6">
					<h3 class="font-heading text-sm">Waktu</h3>
					<div class="-space-y-0.5 tracking-[1.2px]">
						<p>Jumat, 9 Oktober 2026</p>
						<p>08.00-17.00</p>
					</div>
				</div>
				<div class="flex flex-col gap-6">
					<h3 class="font-heading text-sm">Lokasi</h3>
					<div class="-space-y-0.5 tracking-[1.2px]">
						<p>Jero Tegal Sekuta</p>
						<p>Jl. Sekuta Gg. VII No. 9B</p>
						<p>Sanur</p>
					</div>
				</div>
			</div>
			<HeaderFooter variant="1" position="bottom"></HeaderFooter>
		</div>
	</div>
</div>
<div class="relative h-dvh">
	<div class="h-1/2">
		<div class="h-1/2 w-full">
			<img
				src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317210/galeri0.jpg"
				class="h-full w-full object-cover object-center"
				alt=""
			/>
		</div>
	</div>
	<div class="h-1/2 bg-primary"></div>
	<div class="absolute top-1/2 left-0 mx-auto flex w-full -translate-y-1/5 flex-col gap-6">
		<div class="mr-[40%] ml-[20%] aspect-square w-9/20 overflow-hidden">
			<img
				src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317209/galeri1.jpg"
				class=" h-full w-full object-cover object-center"
				alt=""
			/>
		</div>
		<div class="mr-[40%] ml-[20%] aspect-square w-9/20 overflow-hidden">
			<img
				src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317209/galeri2.jpg"
				class=" h-full w-full object-cover object-center"
				alt=""
			/>
		</div>
	</div>
</div>

<div class="h-dvh">
	<div class="flex h-1/2 bg-primary">
		<div class="h-full w-1/2">
			<img
				src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317209/galeri3.jpg"
				class=" h-full w-full object-cover object-center"
				alt=""
			/>
		</div>
		<div class="flex h-full w-1/2 flex-col">
			<div class=" mx-auto my-auto w-3/4">
				<img
					src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317209/galeri4.jpg"
					class=" h-full w-full object-cover object-center"
					alt=""
				/>
			</div>
		</div>
	</div>
	<div class="flex h-1/2 bg-primary">
		<div class="h-full w-1/2"></div>
		<div class="h-full w-1/2">
			<img
				src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317209/galeri5.jpg"
				class=" h-full w-full object-cover object-center"
				alt=""
			/>
		</div>
	</div>
</div>

<div class="relative flex h-dvh bg-white">
	<div class="h-full w-11/20">
		<img
			src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317209/galeri6.jpg"
			class=" h-full w-full object-cover object-left"
			alt=""
		/>
	</div>
	<div class="absolute top-1/2 left-0 flex w-full -translate-y-1/2 flex-col gap-20">
		<div
			class="mr-[20%] ml-[40%]
      aspect-square w-1/3 overflow-hidden"
		>
			<img
				src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317209/galeri7.jpg"
				class=" h-full w-full object-cover object-left"
				alt=""
			/>
		</div>
		<div
			class="mr-[20%] ml-[40%]
      aspect-square w-1/3 overflow-hidden"
		>
			<img
				src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317209/galeri8.jpg"
				class=" h-full w-full object-cover object-left"
				alt=""
			/>
		</div>
		<div
			class="mr-[20%] ml-[40%]
      aspect-square w-1/3 overflow-hidden"
		>
			<img
				src="https://res.cloudinary.com/dzzfgwj4/image/upload/v1789317209/3-3.jpg"
				class=" h-full w-full object-cover"
				alt=""
			/>
		</div>
	</div>
</div>
<Cover></Cover>
