# Asuntoraatali – 360° Scroll Panoraama

Scroll-driven 360° panoraamakatselin, joka pyörii kun sivua scrollataan.

## Demo

Avaa `index.html` selaimessa tai deployaa Verceliin.

## Miten toimii

- **Three.js** renderöi ekvirektilineaarisen panoraamakuvan sfäärille (inside-out sphere)
- **Scroll-tapahtuma** laskee scrollin prosenttiosuuden ja muuttaa sen rotaatiokulmaksi
- **Lerp (lineaarinen interpolointi)** pehmentää liikkeen niin, ettei se ole nykivää
- 1,5× täyskierrosta koko scrollin yli (muutettavissa `ROTATIONS`-vakiosta)

## WordPress / Elementor -integraatio

### Vaihtoehto 1 – Custom HTML widget (nopein)
1. Lisää Elementorissa **Custom HTML** -widget haluamaasi kohtaan sivulle
2. Kopioi `index.html`:n koko sisältö widgettiin
3. Lataa `panorama.png` WordPressin mediaan ja päivitä polku skriptissä

### Vaihtoehto 2 – Shortcode-plugin (suositeltava tuotantoon)
```php
// functions.php tai custom plugin
function asra_panorama_shortcode($atts) {
    $a = shortcode_atts(['image' => '', 'height' => '100vh', 'rotations' => '1.5'], $atts);
    ob_start();
    ?>
    <div class="asra-pano-wrap" style="height:<?= esc_attr($a['height']) ?>">
      <!-- kolme.js + scroll-koodi tässä -->
    </div>
    <?php
    return ob_get_clean();
}
add_shortcode('asra_panorama', 'asra_panorama_shortcode');
```
Käytä sivulla: `[asra_panorama image="https://sinunsivu.fi/wp-content/uploads/panorama.png"]`

### Vaihtoehto 3 – iframe upotus
Yllä olevan staattisen version voi hostata Vercelissä ja upottaa Elementoriin iframe-widgetillä:
```html
<iframe src="https://asraatesti.vercel.app" width="100%" height="600px" frameborder="0"></iframe>
```

## Kehitys

```bash
# Ei build-steppiä tarvita – avaa suoraan selaimella
open index.html

# Tai käytä live-serveriä
npx serve .
```

## Vercel Deploy

Push GitHubiin → linkitä Vercel → automaattinen deploy.
