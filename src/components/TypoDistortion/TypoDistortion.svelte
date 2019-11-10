<script>
  import { onMount } from 'svelte'

  let canvasWrapper

  // TODO: Create own material
  // https://github.com/bradley/Blotter/wiki/Custom-Materials

  function get (src) {
    return new Promise(function (resolve, reject) {
      const el = document.createElement('script')
      el.async = true
      el.src = src

      el.addEventListener('load', function () {
        resolve(src)
      }, false)

      el.addEventListener('error', function () {
        reject(src)
      }, false)

      const nodeToAppendTo = document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]

      nodeToAppendTo.appendChild(el)
    })
  }

  async function loadScripts (scripts) {
    const myPromises = scripts.map(async function (script) {
      return await get(script)
    })

    return await Promise.all(myPromises)
  }

  function loadBlotter() {
    // Load external javascript async:
    // https://zinoui.com/blog/asynchronous-loading-external-javascript
    loadScripts([
      'https://cdnjs.cloudflare.com/ajax/libs/Blotter/0.1.0/blotter.min.js'
    ]).then(function () {
      return loadScripts(['https://cdnjs.cloudflare.com/ajax/libs/Blotter/0.1.0/materials/liquidDistortMaterial.min.js'])
    }).then(function () {
      initBlotter()
    })
  }

  function initBlotter() {
    const text = new Blotter.Text('Lacanau Océan', {
      family : 'sans-serif',
      'font-weight': 'normal',
      size : 120,
      fill : '#171717',
      paddingLeft : 40,
      paddingRight : 40
    })

    const material = new Blotter.LiquidDistortMaterial()
    material.uniforms.uSpeed.value = 0.1
    material.uniforms.uVolatility.value = 0.38
    material.uniforms.uSeed.value = 1.1

    const blotter = new Blotter(material, { texts : text })

    const scope = blotter.forText(text)

    scope.appendTo(canvasWrapper)
  }

  onMount(() => {
    loadBlotter()
  })

</script>

<!-- eslint-disable -->
<style lang='sass'>
	@import './TypoDistortion.scss';
</style>
<!-- eslint-enable -->

<div class="center" bind:this={canvasWrapper}></div>


