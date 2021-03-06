project_path: /web/_project.yaml
book_path: /web/updates/_book.yaml
description: Use Puppeteer to launch Chromium with DevTools features enabled.
{% include "web/_shared/machine-translation-start.html" %}

{# wf_updated_on: 2018-03-05 #}
{# wf_published_on: 2018-01-22 #}
{# wf_tags: devtools #}
{# wf_featured_image: /web/updates/images/generic/chrome-devtools.png #}
{# wf_featured_snippet: Use Puppeteer to launch Chromium with DevTools features enabled. #}
{# wf_blink_components: Platform>DevTools, Internals>Headless #}

{% include "web/tools/chrome-devtools/_shared/styles.html" %}

# DevTools {: .page-title } खोलने के बिना DevTools सुविधाओं का उपयोग करना

{% include "web/_shared/contributors/kaycebasques.html" %}

मैं आमतौर पर "मुझे वास्तव में DevTools की विशेषता एक्स पसंद है, लेकिन यह DevTools बंद करते समय काम करना बंद कर देता है। मैं DevTools बंद होने पर भी सुविधा एक्स को कैसे चला सकता हूं?"

संक्षिप्त जवाब है: आप शायद नहीं कर सकते हैं।

हालांकि, आप * [Puppeteer][puppeteer]{:.external} स्क्रिप्ट को एक साथ हैक कर सकते हैं जो क्रोमियम लॉन्च करता है, एक रिमोट डीबगिंग क्लाइंट खोलता है, फिर आपको पसंद करते हुए DevTools सुविधा को चालू करता है ([क्रोम देवटूल प्रोटोकॉल][CDP] पीआरजीएमएस 1 के माध्यम से) कभी भी स्पष्ट रूप से DevTools खोलने के बिना।

[puppeteer]: https://github.com/GoogleChrome/puppeteer
[CDP]: https://chromedevtools.github.io/devtools-protocol/

उदाहरण के लिए, नीचे दी गई स्क्रिप्ट मुझे व्यूपोर्ट के ऊपरी दाएं भाग पर [एफपीएस मीटर][FPS] ओवरले करने देती है, भले ही DevTools कभी नहीं खुलती है, जैसा कि आप नीचे दिए गए वीडियो में देख सकते हैं।

[FPS]: /web/tools/chrome-devtools/evaluate-performance/reference#fps-meter

    // Node.js version: 8.9.4
    const puppeteer = require('puppeteer'); // version 1.0.0

    (async () => {
      // Prevent Puppeteer from showing the "Chrome is being controlled by automated test
      // software" prompt, but otherwise use Puppeteer's default args.
      const args = await puppeteer.defaultArgs().filter(flag => flag !== '--enable-automation');
      const browser = await puppeteer.launch({
        headless: false,
        ignoreDefaultArgs: true,
        args
      });
      const page = await browser.newPage();
      const devtoolsProtocolClient = await page.target().createCDPSession();
      await devtoolsProtocolClient.send('Overlay.setShowFPSCounter', { show: true });
      await page.goto('https://developers.google.com/web/tools/chrome-devtools');
    })();

<style>  video { width: 100%; } </style>

<video controls>  <source src="https://storage.googleapis.com/webfundamentals-assets/updates/2018/01/devtools.mp4"> </video>

यह केवल कई में से एक है, कई देवटूल की विशेषताएं हैं जिन्हें आप संभावित रूप से क्रोम देवटूल प्रोटोकॉल के माध्यम से एक्सेस कर सकते हैं।

एक सामान्य सुझाव: DevTools प्रोटोकॉल क्लाइंट बनाने का प्रयास करने से पहले [Puppeteer API][API]{:.external} देखें। Puppeteer पहले से ही कई DevTools सुविधाओं के लिए समर्पित एपीआई है, जैसे [कोड कवरेज][coverage] पीआरजीएमएस 1 और [इंटरसेप्टिंग ** कंसोल ** संदेश][console] पीआरजीएमएस 2।

[API]: https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md
[coverage]: https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#class-coverage
[console]: https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#event-console

यदि आपको Puppeteer के माध्यम से DevTools सुविधा तक पहुंचने में सहायता की आवश्यकता है, [स्टैक ओवरफ़्लो पर एक प्रश्न पूछें][SO]{:.external}।

यदि आप एक Puppeteer स्क्रिप्ट को दिखाना चाहते हैं जो DevTools प्रोटोकॉल का उपयोग करता है, तो हमें [@ChromeDevTools][twitter]{:.external} पर ट्वीट करें।

[SO]: https://stackoverflow.com/questions/ask?tags=google-chrome-devtools,puppeteer
[twitter]: https://twitter.com/chromedevtools

{% include "web/_shared/rss-widget-updates.html" %}

{% include "web/_shared/translation-end.html" %}