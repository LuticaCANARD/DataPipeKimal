<script lang="ts">
    import { flip } from 'svelte/animate'
    import type { DisplayCard } from '../../types/view';
    
    const dragDuration = 300
    let cards = Array(20).fill().map((_, i) => i + 1)
    let draggingCard : DisplayCard
    let targetIndex: number
    let lastViewIndex : number;
    $: console.log(draggingCard)
    let animatingCards = new Set()
    
    function tempChange(card) {
        if(lastViewIndex != undefined){
            // 이미 유예된 드래그 중인 카드가 있을 때
            // 이전에 유예된 카드를 원래 위치로 되돌림
        }
        console.log(card)
        animatingCards.add(card)
        setTimeout(() => animatingCards.delete(card), dragDuration)
        const cardAIndex = cards.indexOf(draggingCard)
        const cardBIndex = targetIndex
        console.log(cardBIndex)
        console.log(targetIndex)
        cards[cardAIndex] = cards[cardBIndex]
        console.log(cards[cardBIndex])
        lastViewIndex = cardBIndex
        cards[cardBIndex] = draggingCard
    }
    function decidedChange(){
        console.log("decidedChange")
        console.log(cards)
        console.log(lastViewIndex)
        console.log(targetIndex)
        if(lastViewIndex != targetIndex){
            console.log("change")
            let temp = cards[lastViewIndex]
            cards[lastViewIndex] = cards[targetIndex]
            cards[targetIndex] = temp
            lastViewIndex = targetIndex
        }
    }
</script>

<div class="container">
    {#each cards as card (card)}
    <div
    animate:flip={{ duration: dragDuration }}
    class="card"
    draggable="true"
    on:dragstart={() => draggingCard = card}
    on:dragenter={() => tempChange(card)}
    on:dragend={() => decidedChange()}
    on:dragover|preventDefault
    role="presentation"    
    >
    {card}
</div>
{/each}
</div>

<style>
    .container {
        display: grid;
        grid-template-rows: repeat(4, 1fr);
        grid-template-columns: repeat(5, 1fr);
        gap: 24px;
    }
    
    .card {
        display: flex;
        justify-content: center;
        align-items: center;
        color: darkblue;
        background-color: lightblue;
        width: 100%;
        height: 96px;
        font-size: 1.5rem;
    }
</style>