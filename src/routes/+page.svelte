<script>
  import { getTotalPrizeValueAvailable, lower } from '$lib/utils'
  import { prizeDistribution, tokenPrices } from '$lib/stores'
  import { prizePool, prizeVault } from '$lib/config'
  import { fade } from 'svelte/transition'
  import DepositCard from '$lib/components/deposit/DepositCard.svelte'
  import Loading from '$lib/components/Loading.svelte'

  $: prizeTokenPrice = $tokenPrices[lower(prizePool.prizeToken.address)]
  $: totalPrizeValueAvailable = getTotalPrizeValueAvailable($prizeDistribution ?? [], prizeTokenPrice ?? 0)
  $: formattedTotalPrizeValueAvailable = totalPrizeValueAvailable.toLocaleString('en', {
    maximumFractionDigits: totalPrizeValueAvailable >= 1 ? 0 : 2
  })
</script>

<div class="banner">
  <span class="img-wrapper" />
  <span class="title">
    {#if totalPrizeValueAvailable !== 0}
      <span in:fade>
        Save & win up to <strong>
          {#if prizeVault.asset.isUsdEquivalent}
            ${formattedTotalPrizeValueAvailable}
          {:else}
            {formattedTotalPrizeValueAvailable} {prizeVault.asset.symbol}
          {/if}
        </strong>
      </span>
    {:else}
      <Loading height="1rem" />
    {/if}
  </span>
</div>

<DepositCard />


<style>
  div.banner {
    position: relative;
    width: min(calc(100% - 4rem), 30rem);
    display: flex;
    justify-content: center;
    text-align: center;
    padding: 1rem;
    border-radius: 1rem;
    overflow: hidden;
    isolation: isolate;
  }

  div.banner > span.title {
    padding: 0.25rem 0;
    font-size: 1.6rem;
    font-weight: 700;
  }

  div.banner > span.title strong {
    color: #f0cb6c;
  }

  div.banner > span.img-wrapper {
    position: absolute;
    inset: 0;
    padding-top: 42vw;
    background-repeat: no-repeat;
    background-position-y: center;
    background-size: cover;
    z-index: -10;
  }

  div.banner > span.img-wrapper::after {
    content: '';
    position: absolute;
    inset: 0;
    background-color: #010101;
  }



  @media (min-width: 48rem) {
    div.banner > span.title {
      font-size: 1.75rem;
    }

    div.banner > span.img-wrapper {
      padding-top: 16rem;
    }
  }
</style>
