<script>
	import { onMount } from 'svelte';

	import HistoryTable from '../../template/HistoryTable/HistoryTable.svelte';
	import HistoryTab from '../../template/HistoryTab/HistoryTab.svelte';
	import OrganizerTable from '../../template/OrganizerTable/OrganizerTable.svelte';
	import ReviewTable from '../../template/ReviewTable/ReviewTable.svelte';
	let yearsList,
		history,
		curYear = `2023`;

	// Re-fetch Whenever curYear Changes In HistoryTab Component
	$: fetch(`/history/data/${curYear.trim()}.json`)
		.then((res) => {
			return res.json();
		})
		.then((data) => {
			history = data;
		});
	fetch(`/history/data/years.json`)
		.then((res) => {
			return res.json();
		})
		.then((data) => {
			yearsList = data;
		});

	onMount(()=>{
		document.body.scrollTop = 0;
		document.documentElement.scrollTop = 0;
	});
</script>

<div class="contents">
	<div class="pad">
		<span class="subtitle">기록</span>
		<h1>매년 여러 대회에 참가해 우수한 성적을 거두고 있습니다.</h1>
	</div>
	<img class="pad" src="/res/scoreboard.jpg" alt="2018 Sogang Programming Contest scoreboard" />
	<div class="pad_v">
		<div class="pad_h">
			<h2>필터</h2>
		</div>
		<div class="tabs_container">
			<ul class="tabs pad_h">
				<HistoryTab {yearsList} bind:curYear />
			</ul>
		</div>
	</div>
	<div class="history_contents">
		{#if history}
			{#each history.contests as contest}
				<HistoryTable
					title={contest.title}
					award={contest.award}
					thead={contest.columns}
					tbody={contest.data}
					link={contest.links}
					isReviewAvail={contest.review ? true : false}
				/>
				{#if contest.review}
					<ReviewTable title={'대회 후기'} tbody={contest.review} />
				{/if}
			{/each}
		{/if}
	</div>
	<div class="hr" />
	<div class="pad">
		<b>#</b> = 순위, <b>=</b> = 푼 문제 수 / 점수<br />
		<i class="award winner">&#9733;</i> = 우승,
		<i class="award gold">&#11044;</i> = 금상,
		<i class="award silver">&#11044;</i> = 은상,
		<i class="award bronze">&#11044;</i> = 동상<br />
		<i class="award special">&#11044;</i> = 기타 수상<br />
		<i class="award advanced">&#9650;</i> = 다음 단계 진출<br />
		HM = Honorable Mention<br />
		2019년 이전의 정보는 완전하지 않을 수 있습니다. 정보 등록/수정 요청은 하단의 학회장 메일로 메일을 보내 주세요.
	</div>
</div>
<div class="hr" />
<div class="row pad">
	<div class="p25">
		<h2>역대 스탭</h2>
	</div>
	<div class="p75">
		<div class="table_container">
			<OrganizerTable />
		</div>
		<p style="margin-top: 16px;">
			2018년 이전의 정보는 완전하지 않을 수 있습니다. 정보 등록/수정 요청은 하단의 학회장 메일로 메일을 보내
			주세요.
		</p>
	</div>
</div>

<style>
</style>
