# demystifying-legal-documents-using-gen-AI-new
import { useState, useRef } from 'react';
import { Link, useNavigate } from 'react-router-dom';
import { Upload, FileText, MessageCircle, Shield, Zap, Users } from 'lucide-react';
import { Button } from '@/components/ui/button';
import { Card } from '@/components/ui/card';
import { Image } from '@/components/ui/image';
import { Navigation } from '@/components/ui/navigation';

export default function HomePage() {
  const [dragActive, setDragActive] = useState(false);
  const fileInputRef = useRef<HTMLInputElement>(null);
  const navigate = useNavigate();

  const handleDrag = (e: React.DragEvent) => {
    e.preventDefault();
    e.stopPropagation();
    if (e.type === "dragenter" || e.type === "dragover") {
      setDragActive(true);
    } else if (e.type === "dragleave") {
      setDragActive(false);
    }
  };

  const handleDrop = (e: React.DragEvent) => {
    e.preventDefault();
    e.stopPropagation();
    setDragActive(false);
    
    if (e.dataTransfer.files && e.dataTransfer.files[0]) {
      handleFileUpload(e.dataTransfer.files[0]);
    }
  };

  const handleFileSelect = () => {
    fileInputRef.current?.click();
  };

  const handleFileChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    if (e.target.files && e.target.files[0]) {
      handleFileUpload(e.target.files[0]);
    }
  };

  const handleFileUpload = (file: File) => {
    // Validate file type
    const allowedTypes = ['application/pdf', 'application/msword', 'application/vnd.openxmlformats-officedocument.wordprocessingml.document'];
    if (!allowedTypes.includes(file.type)) {
      alert('Please upload a PDF, DOC, or DOCX file.');
      return;
    }

    // Validate file size (10MB limit)
    const maxSize = 10 * 1024 * 1024; // 10MB in bytes
    if (file.size > maxSize) {
      alert('File size must be less than 10MB.');
      return;
    }

    // Navigate to analysis page with file name
    navigate(/analysis?file=${encodeURIComponent(file.name)});
  };

  return (
    <div className="min-h-screen bg-background">
      {/* Header */}
      <Navigation />

      {/* Hero Section - Inspired by the modular layout */}
      <section className="w-full max-w-[120rem] mx-auto px-6 py-16">
        <div className="grid grid-cols-1 lg:grid-cols-2 gap-8 mb-16">
          {/* Main Hero Card */}
          <Card className="p-12 bg-primary border border-border rounded-3xl">
            <h1 className="text-6xl font-heading font-bold text-primary-foreground mb-6 italic">
              Demystify Legal Documents
            </h1>
            <p className="text-lg font-paragraph text-primary-foreground opacity-90 mb-8">
              Transform complex legal language into clear, understandable insights with our AI-powered analysis platform.
            </p>
            <Link to="/analysis">
              <Button className="bg-buttonbackground text-buttonforeground border border-border rounded-full px-8 py-3 font-paragraph font-medium hover:opacity-90">
                Get Started Today
              </Button>
            </Link>
          </Card>

          {/* AI Badge Card */}
          <Card className="p-12 bg-secondary border border-border rounded-3xl flex items-center justify-center">
            <div className="text-center">
              <Zap className="h-16 w-16 text-secondary-foreground mx-auto mb-4" />
              <h2 className="text-4xl font-heading font-bold text-secondary-foreground italic">
                AI Powered
              </h2>
            </div>
          </Card>
        </div>

        {/* Document Upload Section */}
        <Card className="p-12 bg-secondary border border-border rounded-3xl mb-16">
          <h2 className="text-4xl font-heading font-bold text-secondary-foreground mb-8 italic">
            Upload & Analyze
          </h2>
          
          <div 
            className={`border-2 border-dashed rounded-2xl p-12 text-center transition-colors ${
              dragActive ? 'border-primary bg-primary/10' : 'border-border bg-background'
            }`}
            onDragEnter={handleDrag}
            onDragLeave={handleDrag}
            onDragOver={handleDrag}
            onDrop={handleDrop}
          >
            <Upload className="h-16 w-16 text-secondary-foreground mx-auto mb-6" />
            <h3 className="text-2xl font-heading font-bold text-secondary-foreground mb-4">
              Drop your legal document here
            </h3>
            <p className="text-lg font-paragraph text-secondary-foreground opacity-80 mb-6">
              Supports PDF, DOC, and DOCX files up to 10MB
            </p>
            <input
              ref={fileInputRef}
              type="file"
              accept=".pdf,.doc,.docx,application/pdf,application/msword,application/vnd.openxmlformats-officedocument.wordprocessingml.document"
              onChange={handleFileChange}
              className="hidden"
            />
            <Button 
              onClick={handleFileSelect}
              className="bg-buttonbackground text-buttonforeground border border-border rounded-full px-8 py-3 font-paragraph font-medium hover:opacity-90"
            >
              Choose File
            </Button>
          </div>
        </Card>

        {/* Features Grid */}
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8 mb-16">
          <Card className="p-8 bg-background border border-border rounded-3xl">
            <FileText className="h-12 w-12 text-foreground mb-6" />
            <h3 className="text-xl font-heading font-bold text-foreground mb-4">
              Instant Analysis
            </h3>
            <p className="font-paragraph text-foreground opacity-80">
              Get immediate insights into complex legal terminology and clauses within seconds.
            </p>
          </Card>

          <Card className="p-8 bg-background border border-border rounded-3xl">
            <MessageCircle className="h-12 w-12 text-foreground mb-6" />
            <h3 className="text-xl font-heading font-bold text-foreground mb-4">
              Plain English
            </h3>
            <p className="font-paragraph text-foreground opacity-80">
              Transform legal jargon into clear, understandable language anyone can comprehend.
            </p>
          </Card>

          <Card className="p-8 bg-background border border-border rounded-3xl">
            <Shield className="h-12 w-12 text-foreground mb-6" />
            <h3 className="text-xl font-heading font-bold text-foreground mb-4">
              Secure & Private
            </h3>
            <p className="font-paragraph text-foreground opacity-80">
              Your documents are processed securely and never stored on our servers.
            </p>
          </Card>
        </div>

        {/* CTA Section */}
        <Card className="p-12 bg-primary border border-border rounded-3xl text-center">
          <h2 className="text-4xl font-heading font-bold text-primary-foreground mb-6 italic">
            Ready to Understand Your Documents?
          </h2>
          <p className="text-lg font-paragraph text-primary-foreground opacity-90 mb-8 max-w-2xl mx-auto">
            Join thousands of users who have simplified their legal document review process with our AI-powered platform.
          </p>
          <div className="flex flex-col sm:flex-row gap-4 justify-center">
            <Link to="/analysis">
              <Button className="bg-buttonbackground text-buttonforeground border border-border rounded-full px-8 py-3 font-paragraph font-medium">
                Start Free Analysis
              </Button>
            </Link>
            <Link to="/faq">
              <Button variant="outline" className="border-primary-foreground text-primary-foreground rounded-full px-8 py-3 font-paragraph font-medium hover:bg-primary-foreground hover:text-primary">
                Learn More
              </Button>
            </Link>
          </div>
        </Card>
      </section>
      {/* Footer */}
      <footer className="bg-secondary border-t border-border">
        <div className="max-w-[120rem] mx-auto px-6 py-12">
          <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
            <div>
              <div className="flex items-center space-x-2 mb-4">
                <Shield className="h-6 w-6 text-secondary-foreground" />
                <span className="text-xl font-heading font-bold text-secondary-foreground">Lexora</span>
              </div>
              <p className="font-paragraph text-secondary-foreground opacity-80">
                Making legal documents accessible to everyone through AI-powered analysis and explanation.
              </p>
            </div>
            <div>
              <h4 className="text-lg font-heading font-bold text-secondary-foreground mb-4">Quick Links</h4>
              <div className="space-y-2">
                <Link to="/" className="block font-paragraph text-secondary-foreground opacity-80 hover:opacity-100">Home</Link>
                <Link to="/analysis" className="block font-paragraph text-secondary-foreground opacity-80 hover:opacity-100">Document Analysis</Link>
                <Link to="/dictionary" className="block font-paragraph text-secondary-foreground opacity-80 hover:opacity-100">Legal Dictionary</Link>
                <Link to="/faq" className="block font-paragraph text-secondary-foreground opacity-80 hover:opacity-100">FAQ</Link>
                <Link to="/contact" className="block font-paragraph text-secondary-foreground opacity-80 hover:opacity-100">Contact</Link>
              </div>
            </div>
            <div>
              <h4 className="text-lg font-heading font-bold text-secondary-foreground mb-4">Support</h4>
              <div className="space-y-2">
                <p className="font-paragraph text-secondary-foreground opacity-80">help@lexora.com</p>
                <p className="font-paragraph text-secondary-foreground opacity-80">1-800-lexora</p>
              </div>
            </div>
          </div>
          <div className="border-t border-border mt-8 pt-8 text-center">
            <p className="font-paragraph text-secondary-foreground opacity-60">
              © 2024 Lexora. All rights reserved.
            </p>
          </div>
        </div>
      </footer>
    </div>
  );
}
