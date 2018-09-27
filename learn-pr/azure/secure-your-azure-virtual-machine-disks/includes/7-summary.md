Azure tillhandahåller kryptering för lagringstjänst (SSE) och Azure Disk Encryption (ADE) för att skydda virtuella Azure-diskar. Dessa tekniker samverkar för att ge stark 256-bitars kryptering som ett led i en djupskyddsmetod för Virtuella Azure-diskar. Du måste ha slutfört förhandskraven för Azure Disk Encryption för att kunna aktivera diskkryptering. Konfigurationsskriptet som krävs för Azure Disk Encryption kan automatisera den här processen. När du aktiverar kryptering på nya virtuella datorer kan du använda en Azure Resource Manager-mall. Detta säkerställer att dina data är krypterade vid distributionspunkten, vilket eliminerar alla sårbarheter.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="additional-resources"></a>Ytterligare resurser

- [Felsökning för Azure VM-diskkryptering](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-tsg)
- [Hur man krypterar en virtuell Linux-dator i Azure](https://docs.microsoft.com/azure/virtual-machines/linux/encrypt-disks)
- [Översikt av Azure-diskkryptering ](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview)
- [Vilka Linux-distributioner stöder Azure-diskkryptering?](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-faq#bkmk_LinuxOSSupport)
- [Resource Manager-mallar på GitHub](https://github.com/Azure/azure-quickstart-templates)