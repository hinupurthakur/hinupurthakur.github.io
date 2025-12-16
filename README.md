
# Nupur Thakur

**Last seen building platforms at CRED**

📍 Bengaluru, IN | 📧 hinupurthakur@gmail.com 
[LinkedIn](https://linkedin.com/in/hinupurthakur) | [GitHub](https://github.com/hinupurthakur)

<button id="downloadPdfBtn" style="display: flex; align-items: center; gap: 8px; padding: 10px 16px; background-color: #2563eb; color: white; border: none; border-radius: 6px; cursor: pointer; font-size: 14px; font-weight: 500; margin-bottom: 20px;">
  <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
    <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
    <polyline points="7 10 12 15 17 10"></polyline>
    <line x1="12" y1="15" x2="12" y2="3"></line>
  </svg>
  Download as PDF
</button>

<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<script>
document.getElementById('downloadPdfBtn').addEventListener('click', async function() {
  const btn = this;
  btn.disabled = true;
  btn.textContent = 'Generating PDF...';
  
  try {
    // Get the markdown content
    let mdContent = document.querySelector('main')?.innerText || 
                    document.querySelector('article')?.innerText ||
                    document.querySelector('.content')?.innerText;

    if (!mdContent) {
      alert('No content found');
      return;
    }

    // Remove embedded HTML but keep URLs
    mdContent = mdContent
      .replace(/<iframe[^>]*>[\s\S]*?<\/iframe>/gi, '[Embedded Content]')
      .replace(/<embed[^>]*>/gi, '[Embedded Content]')
      .replace(/<video[^>]*>[\s\S]*?<\/video>/gi, '[Embedded Video]')
      .replace(/<audio[^>]*>[\s\S]*?<\/audio>/gi, '[Embedded Audio]');

    // Create temporary element for PDF conversion
    const tempDiv = document.createElement('div');
    tempDiv.style.padding = '20px';
    tempDiv.style.backgroundColor = 'white';
    tempDiv.style.color = '#000';
    tempDiv.style.fontSize = '14px';
    tempDiv.style.lineHeight = '1.6';
    tempDiv.style.position = 'absolute';
    tempDiv.style.left = '-9999px';
    tempDiv.style.width = '800px';
    tempDiv.innerHTML = `<pre style="white-space: pre-wrap; word-wrap: break-word; font-family: serif;">${mdContent}</pre>`;
    
    document.body.appendChild(tempDiv);

    const canvas = await html2canvas(tempDiv, {
      scale: 2,
      backgroundColor: '#ffffff'
    });

    document.body.removeChild(tempDiv);

    // Create PDF using jsPDF
    const { jsPDF } = window.jspdf;
    const pdf = new jsPDF({
      orientation: 'portrait',
      unit: 'mm',
      format: 'a4'
    });

    const imgData = canvas.toDataURL('image/png');
    const pdfWidth = pdf.internal.pageSize.getWidth();
    const pdfHeight = pdf.internal.pageSize.getHeight();
    const imgWidth = pdfWidth - 20;
    const imgHeight = (canvas.height * imgWidth) / canvas.width;

    let heightLeft = imgHeight;
    let position = 10;

    pdf.addImage(imgData, 'PNG', 10, position, imgWidth, imgHeight);
    heightLeft -= pdfHeight - 20;

    while (heightLeft > 0) {
      position = heightLeft - imgHeight;
      pdf.addPage();
      pdf.addImage(imgData, 'PNG', 10, position, imgWidth, imgHeight);
      heightLeft -= pdfHeight - 20;
    }

    // Download
    const filename = document.title || 'document';
    pdf.save(`${filename}.pdf`);
  } catch (error) {
    console.error('Error generating PDF:', error);
    alert('Error generating PDF: ' + error.message);
  } finally {
    btn.disabled = false;
    btn.innerHTML = '<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path><polyline points="7 10 12 15 17 10"></polyline><line x1="12" y1="15" x2="12" y2="3"></line></svg> Download as PDF';
  }
});
</script>
---

## Summary

Engineer with a knack for curiosity, experienced in building distributed systems and robust APIs. Skilled in cloud infrastructure, optimizing system performance and implementing CI/CD pipelines across cloud platforms.

---

## Professional Skills

**Languages & Frameworks:** C, C++, Go, Python, Java, TypeScript, NodeJS

**Infrastructure as Code:** Pulumi, CloudFormation, Terraform

**Configuration Management:** Ansible

**Databases:** PostgreSQL, MongoDB, MySQL, SQLite, OrientDB

**Cloud Platforms:** AWS, GCP

**CI/CD & Tools:** Git, Jenkins, GitHub Actions, Bitbucket Pipelines

**Orchestration Tools:** ECS, Kubernetes (EKS, GKE), [Docker Swarm](https://github.com/tiangolo/dockerswarm.rocks)

---

## Work Experience

### CRED | Bengaluru, IN
**Site Reliability Engineering (Platform)** | *May 2022 - Present*

- Ownership of all platform tools and CI/CD, enabling engineering teams to be self-served and saving 2x effort
- Engineered automated [GitHub Actions Runner Controller](https://github.com/actions/actions-runner-controller) (ARC) with Pulumi IaC and Karpenter-based nodepools on EKS, slashing setup effort by 90% and reducing job scheduling latency by 80% through optimized pod provisioning and dynamic auto-scaling
- Re-engineered deployment pipeline with multi-layered optimizations including [Gradle remote cache for Java services](https://docs.gradle.org/current/userguide/build_cache.html), [Docker Buildx caching](https://docs.docker.com/build/cache/backends/s3/), parallel CI stages, automated PR checks, and streamlined health checks, reducing average service deployment time by 50% (from 18 min to 9 min)
- Designed and implemented an in-house cost attribution tool, ensuring 100% automated tagging of microservices
- Re-engineered [CI/CD tool](https://engineering.cred.club/scaling-up-with-caterpillar-the-codepipeline-at-cred-942dd818e131) into high-performance, platform-agnostic tool supporting multiple pipeline engines, replacing legacy shell scripts with optimized Golang binary, designing complete Pulumi IaC suite, and building comprehensive testing framework; cutting deployment times by 50% and dramatically improving developer productivity and DevEx
- Developed a self-service [Golang CLI](https://github.com/spf13/cobra) tool to generate configuration for microservice CI/CD pipelines, improving 2x developer productivity
- Set up infrastructure of a new organization from scratch including AWS org, network stack, [VPN](https://pritunl.com), CI/CD platform and other infra tools; also set up multi-org artifact replication

### Gather Network | Gurgaon, IN
**Software Engineer II** | *Mar 2021 - May 2022*

- Worked as founding engineer, developing tools from 0 to 1 including [Golang APIs](https://github.com/gorilla/mux), [CLI tools](https://github.com/spf13/cobra) and [desktop applications](https://github.com/fyne-io/fyne)
- Led DevSecOps initiatives, setting up infrastructure using Terraform for all CI/CD deployments and implementing [SAST analysis](https://semgrep.dev) in CI
- Contributed to obtaining certifications including ISO 27001
- Built end-to-end Ansible playbook to orchestrate blockchain genesis nodes

### RemoteState | Noida, IN
**Software Engineer** | *May 2020 - Mar 2021*

- Developed and integrated multiple software APIs in Golang, designing APIs from ground up and enabling seamless communication between services
- Led cross-functional team collaboration during project build phase, coordinating across engineering, QA, and product teams to meet a strict 6-month launch deadline
- Facilitated active customer feedback loop post-launch, collecting insights that drove iterative improvements resulting in 30% increase in user satisfaction scores within six months and 20+ new client onboarding

### Anusaaraka Lab, IIITH | Hyderabad, IN
**Research Intern** | *Jun 2019 - Dec 2019*

[Read the Report](https://docs.google.com/document/d/1tlBGITVYUHt6-AQgiuX2A1UdLPkSUV6T51IFr-l_2Fs/edit?tab=t.0)

---

## Open Source Contributions

- **terraform-aws-modules/terraform-aws-cloudfront** - [feat: Added support for response headers policy](https://github.com/terraform-aws-modules/terraform-aws-cloudfront/pull/57)
- **meshery/meshery** - [mesherctl : Resolve user empty error-WHOAMI declared](https://github.com/meshery/meshery/pull/663)
- **meshery/meshery** - [mesheryctl : Use of sudo wisely for enhancement of the meshery script](https://github.com/meshery/meshery/pull/680)
- **meshery/meshery** - [mesheryctl: Change in the value of MESHERY_VERSION variable](https://github.com/meshery/meshery/pull/818)

---

## Talks & Presentations

### Women Who Go, Bengaluru | *June 2025*
**Go x CRED Platform Engineering**  
<iframe src="https://www.linkedin.com/embed/feed/update/urn:li:share:7340464040940314626?collapsed=1" height="670" width="504" frameborder="0" allowfullscreen="" title="Embedded post"></iframe>

### Layer5 | *October 2020*
**Golang with mesheryctl**  
<!-- [Watch the talk](https://www.youtube.com/watch?v=wK7Q-zbJ3gQ) -->
<iframe width="560" height="315" src="https://www.youtube.com/embed/wK7Q-zbJ3gQ?si=JApp6XFEHp91qsPh" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
---

## Education

**Banasthali Vidyapith, Rajasthan**  
Bachelor of Technology in Computer Science | *2016 - 2020*

---

*Last Updated: December 2025*
